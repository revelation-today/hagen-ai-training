# Production Concerns — Bound It, Trace It, Cost It, Test It, Gate It

An agent that works in a notebook and an agent you would let near a real system are different artifacts. This file is the difference. Five controls, in the order you should build them.

---

## 1. Bound what it may do

Three kinds of bound, and you need all three. Missing any one is how the interesting incidents happen.

### 1a. Bound the tools

**The tool list is the permission model.** Whatever is in it, the agent can do — at any point, in any order, on any input, including inputs an outsider wrote. Design it accordingly.

| Rule | Why |
|---|---|
| **Read-only first.** Ship a read-only agent, measure it for weeks, add writes later — or never | Removes an entire failure class at zero cost to the learning |
| **Least privilege at the server, not the prompt** | Session 11's rule, and it is load-bearing here. A system prompt saying "never touch prod" is a suggestion; a service account without prod access is a control |
| **One tool = one narrow capability** | `run_sql(query)` is not a tool, it is a shell. `get_release_contents(release_id)` is a tool |
| **No tool whose blast radius you would not accept from an automated caller with no judgement** | Because that is exactly what it is |
| **Separate the identity that reads untrusted content from the identity that holds credentials** | The cheapest structural mitigation available. Session 14 explains why |

### 1b. Bound the loop

```python
"""Bounding an agent run. These caps are not optional."""

import time
from dataclasses import dataclass, field


@dataclass
class Budget:
    """Every agent run needs all four caps. A missing cap is an incident
    waiting for the right input."""
    max_steps: int = 8
    max_tokens: int = 60_000        # input + output, summed across the run
    max_seconds: float = 120.0
    max_tool_calls: dict = field(default_factory=lambda: {"open_ticket": 0})
    # ^ per-tool caps. 0 = present in the schema but not permitted this run.

    started: float = field(default_factory=time.monotonic)
    tokens_used: int = 0
    calls_made: dict = field(default_factory=dict)

    def check(self, step: int) -> str | None:
        """Return a stop reason, or None to continue."""
        if step > self.max_steps:
            return f"step cap ({self.max_steps})"
        if self.tokens_used > self.max_tokens:
            return f"token cap ({self.max_tokens}, used {self.tokens_used})"
        if time.monotonic() - self.started > self.max_seconds:
            return f"time cap ({self.max_seconds}s)"
        return None

    def allow_tool(self, name: str) -> bool:
        limit = self.max_tool_calls.get(name)
        if limit is None:
            return True                      # unlisted tools are unlimited
        return self.calls_made.get(name, 0) < limit


b = Budget(max_steps=3, max_tokens=1000)
print(b.check(step=1))                       # Expected: None
b.tokens_used = 1200
print(b.check(step=1))                       # Expected: token cap (1000, used 1200)
print(b.check(step=4))                       # Expected: step cap (3)
print(b.allow_tool("get_diff"))              # Expected: True
print(b.allow_tool("open_ticket"))           # Expected: False   (limit is 0)

# The important part is not this class. It is that hitting a cap is a
# NORMAL, EXPECTED, LOGGED OUTCOME -- not an exception -- and that the
# rate at which each cap fires is a metric you watch.
```

**A cap that fires is information, not a bug.** If the step cap fires on 30% of runs, either your task is not agentic, your tools are wrong, or your cap is too low — and you cannot tell which without the number.

### 1c. Bound the consequences

Everything the agent does should be reversible, or gated, or both:

- Writes go to a **staging artifact** a human promotes — a draft, a branch, a proposed change — not to the live system.
- Every action is **logged with the trace ID that produced it**, so "why did this happen?" is answerable next quarter.
- **No irreversible action without an explicit human approval**, and "explicit" means a person who saw the specific action, not a person who approved the project.

## 2. Trace it — observability is not optional here

For a single call, logging input and output is enough. For an agent it is nowhere near enough, because the interesting question is never "what did it say?" — it is **"why did it do that at step 5?"**

**Log per step:** the step number, the model's text (thought), the tool name and arguments, the tool result *and whether it errored*, input/output tokens, latency, and cumulative totals. **Log per run:** a run ID, the goal, the total steps, total tokens, total cost, the stop reason, and the final output.

```python
"""Minimal structured tracing. One JSON line per step, one per run.
Not a framework -- deliberately. Everything here is the standard
observability you already do, applied per step."""

import json
import time
import uuid


class Trace:
    def __init__(self, goal: str):
        self.run_id = str(uuid.uuid4())[:8]
        self.goal = goal
        self.started = time.monotonic()
        self.steps: list[dict] = []

    def step(self, n, thought, tool, args, result, is_error, tok_in, tok_out):
        rec = {
            "run_id": self.run_id, "step": n,
            "thought": (thought or "")[:400],
            "tool": tool, "args": args,
            "result": str(result)[:400], "is_error": is_error,
            "tokens_in": tok_in, "tokens_out": tok_out,
            "elapsed_s": round(time.monotonic() - self.started, 2),
        }
        self.steps.append(rec)
        print(json.dumps(rec))               # in production: to your log sink

    def finish(self, stop_reason: str, output: str) -> dict:
        summary = {
            "run_id": self.run_id, "goal": self.goal,
            "steps": len(self.steps),
            "tokens_in": sum(s["tokens_in"] for s in self.steps),
            "tokens_out": sum(s["tokens_out"] for s in self.steps),
            "tool_errors": sum(1 for s in self.steps if s["is_error"]),
            "stop_reason": stop_reason,
            "elapsed_s": round(time.monotonic() - self.started, 2),
            "output": output[:600],
        }
        print(json.dumps(summary))
        return summary


t = Trace("Why did helios-audio regress in 2.6?")
t.step(1, "Need the diff first.", "get_diff",
       {"component": "helios-audio"}, {"files": 2}, False, 1180, 96)
t.step(2, "Small diff, read the buffer file.", "get_file_change",
       {"path": "audio_buf.c"}, {"change": "4096 -> 1024"}, False, 1410, 88)
summary = t.finish("end_turn", "AUDIO_BUF_SZ reduced 4x in CR-8817.")

print(summary["steps"], summary["tokens_in"], summary["tool_errors"])
# Expected output: 2 2590 0
```

Four metrics to put on a dashboard from day one, because they are the ones that move before anything breaks visibly:

| Metric | What a change in it tells you |
|---|---|
| **Steps per run** (median and p95) | A rising p95 means the agent is wandering — usually a model change or a degraded tool |
| **Tool error rate** | Rising means a tool broke, or the model is calling it with arguments the schema permits and the tool does not |
| **Cap-fire rate**, per cap | Which constraint actually binds. Often not the one you expected |
| **Cost per successful task** | The number that decides whether this stays in production |

Open-source tracing tooling exists and is worth adopting once you outgrow print statements (Arize Phoenix and similar, MIT/Apache — `resources/sources.md`). The principle is the same either way: **you will debug agents through traces, and traces you did not capture do not exist.**

## 3. Cost and latency — pick up Session 2's meter

Session 2 established the mechanism: each step re-sends the whole accumulated trace, so **one user request becomes N billed calls with growing inputs**. For an 8-step agent with a 2,000-token base prompt, ~600 tokens added per step, and 300 output tokens per step, that was **8 calls, 32,800 input tokens, ~13× the cost of a single call.**

| Lever | Effect | Cost of pulling it |
|---|---|---|
| **Cache the stable prefix** (system prompt, tool schemas) | Up to ~90% off the repeated input portion — the largest single lever for agents, because the prefix is re-sent every step | Restructure so stable content comes first and variable content last. Verify with cache-hit metrics |
| **Cap steps aggressively** | Linear, immediate | Some hard tasks fail. Measure how many |
| **Use a smaller model for execution steps** | ~3× on the pattern in `04` §1 | A planner/executor split to build and maintain |
| **Return small tool results** | Compounding — every observation is re-sent every subsequent step | Tool redesign: summary plus a handle for detail |
| **Prune or summarise old trace entries** | Compounding | You may prune the observation that mattered. Test it |
| **Don't build the agent** (`05`) | 100% | The one people skip |

**Latency behaves worse than cost.** Cost is roughly linear in steps; *perceived* latency is the p95 of a distribution with a long tail, because the step count is decided at run time. An agent with a median of 8 seconds and a p95 of 90 seconds is a bad interactive experience regardless of the median. **Design for the tail: stream progress, show the current step, and make cancellation work.**

## 4. Non-determinism and testing

Here is the uncomfortable part. **The same input produces a different trajectory.** Not just different wording — a different number of steps, different tools, in a different order. Temperature 0 reduces variance; it does not deliver determinism, and it does not touch the variance introduced by the tools' own changing data.

So the ordinary test — "given input X, assert output Y" — does not apply. What replaces it:

### 4a. Assert on properties, not trajectories

```python
"""Testing a non-deterministic agent. Assert on invariants, never on
the exact path taken."""

def check_run(summary: dict, trace_steps: list[dict]) -> list[str]:
    """Return a list of violations. Empty list = pass."""
    v = []

    # -- SAFETY invariants: must hold on every run, no exceptions --------
    write_tools = {"open_ticket", "apply_config", "send_email"}
    used = {s["tool"] for s in trace_steps if s["tool"]}
    if used & write_tools:
        v.append(f"SAFETY: write tool used: {used & write_tools}")
    if summary["steps"] > 8:
        v.append(f"SAFETY: {summary['steps']} steps exceeds cap")
    if summary["tokens_in"] + summary["tokens_out"] > 60_000:
        v.append("SAFETY: token budget exceeded")

    # -- QUALITY properties: the answer's shape, not its wording ---------
    out = summary["output"]
    if "CR-" not in out and "INSUFFICIENT EVIDENCE" not in out:
        v.append("QUALITY: no ticket ID and no explicit uncertainty")
    if "NOT VERIFIED" not in out and "INSUFFICIENT EVIDENCE" not in out:
        v.append("QUALITY: did not state what it failed to verify")

    # -- GROUNDING: every ticket ID in the answer must come from a tool --
    import re
    claimed = set(re.findall(r"CR-\d+", out))
    observed = set(re.findall(r"CR-\d+", " ".join(s["result"] for s in trace_steps)))
    if claimed - observed:
        v.append(f"GROUNDING: invented ticket IDs {claimed - observed}")

    return v


good = {"steps": 3, "tokens_in": 4280, "tokens_out": 325,
        "output": "AUDIO_BUF_SZ reduced 4x in CR-8817. NOT VERIFIED: no test run."}
steps = [{"tool": "get_diff", "result": "{'files': 2}"},
         {"tool": "get_file_change", "result": "{'ticket': 'CR-8817'}"}]
print(check_run(good, steps))
# Expected output: []

bad = {"steps": 3, "tokens_in": 4280, "tokens_out": 325,
       "output": "Caused by CR-9999. NOT VERIFIED: no test run."}
print(check_run(bad, steps))
# Expected output: ["GROUNDING: invented ticket IDs {'CR-9999'}"]
```

That **GROUNDING** check is the highest-value assertion in the file: it verifies mechanically that every identifier the agent asserted actually appeared in a tool result. It catches confident fabrication without needing a model to judge, and it is cheap.

### 4b. Run each case several times

One pass is not evidence. Run each case **k times** and report the pass^k rate — success on *all* k attempts. Expect it to sit well below the single-run rate; the literature puts the gap at **15–25 points** (`06` §5). Reporting only the best run is how a 70%-reliable system gets described as 90%.

### 4c. Twenty tasks from real failures

Session 11's discipline transfers directly. **Twenty to fifty real tasks, drawn from things that actually went wrong**, beats a synthetic benchmark and defeats the "we need a giant eval set before we can start" paralysis. Version the suite with the agent. Re-run it on every model change, every prompt change, and every tool change — all three break agents, and only one of them is under your control.

### 4d. Grade in tiers

| Tier | Cost | Use for |
|---|---|---|
| Deterministic rules (the code above) | Free | Every safety invariant, every grounding check, every budget assertion |
| Model-as-judge | Cheap, noisy — and **itself an unvalidated prompt**; calibrate it against human labels before trusting it | Fuzzy quality properties |
| Human review of traces | Expensive, authoritative | A sample every release; every failure of tier 1 |

Push everything as far left as it will go.

## 5. Failure modes to watch for by name

| Failure | Symptom in the trace | Control |
|---|---|---|
| **Looping** | The same tool with the same arguments, repeatedly | Step cap; detect and break on repeated (tool, args); tell the model it already tried that |
| **Premature stop** | Confident answer at step 2, wrong | Explicit success criteria in the prompt; require named evidence |
| **Tool thrash** | Alternating between two near-identical tools | Merge or clearly differentiate the tool descriptions |
| **Silent tool failure** | A tool returns `{}` or `null` and the model reasons from it | `is_error: true`; never return bare empties |
| **Context poisoning** | One wrong observation at step 2 is cited as fact at steps 3–8 | Grounding checks; short runs; a human at the boundary |
| **Goal drift** | Ends up solving an adjacent, more interesting problem | Restate the goal each step; assert the output answers the original question |
| **Cap thrash** | Cap fires on most runs | Not a cap problem. The task is probably not agentic (`05`) |

## 6. The human gate

The single control that makes everything above defensible.

> **No automated pipeline acts on model output without a qualified human gate.**

Three words carry the weight:

- **Qualified** — a person competent to evaluate *this specific output*, not someone in the org chart. Session 13's finding applies with force: human-in-the-loop is **necessary and not sufficient**, and you must also ask whether the human is *equipped* to catch the error.
- **Gate** — the action does not proceed without approval. Not a notification. Not an audit log reviewed monthly. A blocking step.
- **Acts** — the gate belongs where output becomes action. Read-only agents need review; acting agents need a gate.

And the trap this course keeps returning to: **the better the agent gets, the harder the gate is to staff.** A human approving a stream of correct actions stops reading. That is not a character flaw; system safety has known for decades that people are poor at catching infrequent automation errors. Which means the gate must be **designed to be checkable, not merely present**: small diffs, explicit evidence, a stated list of what was *not* verified, and a default of "reject" when the trace is unclear.

**Design implication:** an agent whose output is a 40-line proposed change with cited evidence is gateable. An agent whose output is "done ✅" is not, no matter who is standing next to it.

## 7. The hand-off to Session 14

Everything in this file is about correctness, cost, and control. There is one more thing, and it is a whole session.

The system-safety framework this course inherited lists the ways a hazard gets triggered. One of its **initiating mechanisms** is:

> **an API that directly acts on LLM output.**

That bullet was written before agents were common, and it is exactly right. **An agent is that bullet, built on purpose.** It is a language model whose output is wired directly into an API that acts.

Which means every property that makes an agent useful is also the property that makes it the highest-risk deployment shape in this course:

- It reads content it did not author — tickets, logs, diffs, documents — some of which an outsider can influence.
- It holds credentials and tools.
- It acts without a human between the decision and the consequence.
- Its loop feeds observations back into its own context, so **a single poisoned observation influences every subsequent step.**

Name that. Then stop. **Session 14 does the security** — prompt injection direct and indirect, the three-precondition test, excessive agency, and what to actually do about it. Read that session before you connect an agent to anything you care about, not after.

---

## What to remember

- **Bound three things: the tools, the loop, the consequences.** Step, token, time, and per-tool caps — all four, and a cap firing is a logged metric, not an exception.
- **Least privilege in code, never in a prompt.**
- **Trace every step from day one.** Steps per run, tool error rate, cap-fire rate, cost per successful task.
- **Prompt caching is the largest cost lever for agents**, because the stable prefix is re-sent on every step.
- **Latency is a distribution with a tail.** Design for the p95: stream progress, allow cancellation.
- **Test properties, not trajectories.** Safety invariants, grounding checks, twenty real tasks, k runs each, pass^k reported.
- **The human gate must be qualified, blocking, and checkable** — and the better the agent gets, the harder that is.
- **An agent is "an API acting on model output."** That is the hazard. Session 14.

---

**Next:** `99-key-takeaways.md`.
