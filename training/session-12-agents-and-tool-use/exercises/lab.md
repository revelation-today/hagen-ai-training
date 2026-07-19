# Lab — Build a ReAct Agent, Then Break It

**Time:** ~25–30 minutes · **Environment:** Google Colab (first choice), JupyterLite (fallback), or any local Python 3.10+
**Two tracks:** a **coding track** (Parts 1–4) and a **no-code track** (Part 0) for participants who do not write Python. Both finish at the same debrief.

> ⚠️ **Verify at delivery.** Model IDs, SDK parameter names, and per-token prices drift. Every one of them is set in a single constant near the top of the notebook, on purpose.

---

## Part 0 — The no-code track (~20 min)

You do not need to run anything to get the value of this lab. Work through `content/03` §2 — the annotated trace — and answer these five questions in writing. They are the same questions the coding track answers with code.

1. **Input tokens grow from 1,180 to 1,410 to 1,690 across three steps while the question never changes. Why?** *(Expected: the whole accumulated trace is re-sent on every call. This is the cost multiplier, made visible.)*
2. **Step 2's thought says "small diff, read it directly." What would step 2 have been if the diff had been 4,000 lines?** *(Expected: something else entirely — bisect, or narrow by file. Naming that is naming adaptation, which is the property that justified building an agent.)*
3. **The final answer says "I did not run a test to confirm it." Where did that sentence come from?** *(Expected: the system prompt asked for it. Nothing in the mechanism produces it otherwise.)*
4. **Suppose the `get_diff` tool silently returned `{}` instead of an error. Read the trace forward and describe what happens.** *(Expected: the model reasons from nothing, and — having no legal way to be uncertain unless you gave it one — produces a confident answer built on an absence. This is failure mode #5 in `content/07`.)*
5. **Write three assertions you could make about *any* run of this agent that do not depend on the exact path it took.** *(Expected: no write tool was used; step count under the cap; every ticket ID in the answer appeared in a tool result. That third one is the grounding check.)*

Bring your answer to question 5 to the debrief. It is the same artifact the coding track produces in Part 4.

---

## Part 1 — Setup (~4 min)

**Colab:** open a new notebook. **JupyterLite fallback** (no install, browser only): note that network calls to a model API may be blocked, in which case run the *offline mode* below, which exercises the loop against a scripted fake model and still teaches everything except the model's actual behaviour.

```python
!pip -q install anthropic
```

```python
import os, json, time
from anthropic import Anthropic

os.environ["ANTHROPIC_API_KEY"] = "..."   # or use Colab's secrets panel
client = Anthropic()

MODEL = "claude-opus-4-8"    # ⚠️ verify at delivery
MAX_STEPS = 6
```

**Offline mode.** If you have no key or no network, replace the model call in Part 3 with a scripted stub that returns a fixed sequence of tool calls. You lose the model's judgement; you keep the loop, the tool execution, the error handling, and every assertion in Part 4. That is most of the lab.

---

## Part 2 — Tools and schemas (~6 min)

Type this in. Do not paste it — the schemas are the part people skim, and they are half of agent quality.

```python
_DIFFS = {
    ("helios-audio", "2.5", "2.6"): {
        "files": 2, "lines_changed": 41,
        "paths": ["audio_buf.c", "audio_cfg.h"],
    },
}
_CHANGES = {
    "audio_buf.c": {"change": "AUDIO_BUF_SZ 4096 -> 1024", "ticket": "CR-8817"},
    "audio_cfg.h": {"change": "added HELIOS_AUDIO_DEBUG flag", "ticket": "CR-8802"},
}

def get_diff(component: str, from_rel: str, to_rel: str) -> dict:
    return _DIFFS.get((component, from_rel, to_rel), {"error": "no such diff"})

def get_file_change(path: str) -> dict:
    return _CHANGES.get(path, {"error": f"no recorded change for {path}"})

TOOL_IMPLS = {"get_diff": get_diff, "get_file_change": get_file_change}

TOOLS = [
    {
        "name": "get_diff",
        "description": (
            "Return a summary of what changed in one component between two "
            "release versions: how many files and lines changed, and which "
            "file paths. Call this FIRST when investigating a regression, "
            "before looking at any individual file."
        ),
        "input_schema": {
            "type": "object",
            "properties": {
                "component": {"type": "string", "description": "e.g. 'helios-audio'."},
                "from_rel":  {"type": "string", "description": "Older release, e.g. '2.5'."},
                "to_rel":    {"type": "string", "description": "Newer release, e.g. '2.6'."},
            },
            "required": ["component", "from_rel", "to_rel"],
            "additionalProperties": False,
        },
    },
    {
        "name": "get_file_change",
        "description": (
            "Describe the specific change made to one file, with its change "
            "request ID. Only call this for a path returned by get_diff."
        ),
        "input_schema": {
            "type": "object",
            "properties": {"path": {"type": "string", "description": "Path from get_diff."}},
            "required": ["path"],
            "additionalProperties": False,
        },
    },
]

SYSTEM = """You investigate software regressions using the tools provided.

Procedure:
1. Get the diff between releases before looking at individual files.
2. Read only files the diff actually names.
3. Stop as soon as you can name a concrete change that plausibly explains
   the regression. Do not keep exploring for completeness.

Rules:
- Do NOT invent file paths, ticket IDs, or version numbers. Use only values
  that appear in a tool result.
- If the tools do not give you enough to identify a cause, say
  "INSUFFICIENT EVIDENCE" and state exactly what you would need.
- In your final answer, state explicitly what you did NOT verify.
"""

print(get_diff("helios-audio", "2.5", "2.6"))
# Expected: {'files': 2, 'lines_changed': 41, 'paths': ['audio_buf.c', 'audio_cfg.h']}
print(get_diff("helios-video", "2.5", "2.6"))
# Expected: {'error': 'no such diff'}
```

---

## Part 3 — The loop (~8 min)

```python
def run_agent(question: str, max_steps: int = MAX_STEPS):
    """Returns (final_text, trace). The trace is the artifact you test."""
    messages = [{"role": "user", "content": question}]
    trace = []

    for step in range(1, max_steps + 1):
        r = client.messages.create(
            model=MODEL, max_tokens=2000,
            system=SYSTEM, tools=TOOLS, messages=messages,
        )

        thought = " ".join(b.text for b in r.content if b.type == "text").strip()
        calls = [b for b in r.content if b.type == "tool_use"]
        print(f"[{step}] stop={r.stop_reason} tools={[c.name for c in calls]} "
              f"in={r.usage.input_tokens} out={r.usage.output_tokens}")
        if thought:
            print(f"     thought: {thought[:110]}")

        if r.stop_reason != "tool_use":
            trace.append({"step": step, "thought": thought, "tool": None,
                          "result": "", "is_error": False,
                          "tokens_in": r.usage.input_tokens,
                          "tokens_out": r.usage.output_tokens})
            return thought, trace

        messages.append({"role": "assistant", "content": r.content})
        results = []
        for c in calls:
            fn = TOOL_IMPLS.get(c.name)
            if fn is None:
                out, err = {"error": f"unknown tool {c.name}"}, True
            else:
                try:
                    out, err = fn(**c.input), False
                except Exception as exc:                  # noqa: BLE001
                    out, err = {"error": f"{type(exc).__name__}: {exc}"}, True
            print(f"     action:  {c.name}({c.input}) -> {out}")
            trace.append({"step": step, "thought": thought, "tool": c.name,
                          "result": json.dumps(out), "is_error": err,
                          "tokens_in": r.usage.input_tokens,
                          "tokens_out": r.usage.output_tokens})
            results.append({"type": "tool_result", "tool_use_id": c.id,
                            "content": json.dumps(out), "is_error": err})
        messages.append({"role": "user", "content": results})

    return f"STOPPED: step cap ({max_steps}) reached.", trace


answer, trace = run_agent("Why did helios-audio regress between 2.5 and 2.6?")
print("\n---\n", answer)

# Expected shape (wording varies run to run — that variability IS the lesson):
# [1] stop=tool_use tools=['get_diff'] in=~1180 out=~96
# [2] stop=tool_use tools=['get_file_change'] in=~1410 out=~88
# [3] stop=end_turn tools=[] in=~1690 out=~141
# ---
#  ...AUDIO_BUF_SZ reduced from 4096 to 1024 in CR-8817...
#  NOT VERIFIED: no test was run to confirm the link.
```

**Before moving on, answer two questions from your own output:**

1. How many input tokens did the whole run use, and how does that compare with the ~600 a single non-agent call on this question would take? *(Sum `tokens_in` across the trace.)*
2. Did the final answer state what it did not verify? If not, why not — and what would you change?

---

## Part 4 — Now break it (three challenges, ~8 min)

Pick at least two. Each one is a failure mode from `content/07`.

### Challenge A — silent tool failure

Change `get_diff` so that on an unknown component it returns `{}` instead of `{"error": ...}`. Run `run_agent("Why did helios-video regress between 2.5 and 2.6?")`.

**Watch for:** whether the model notices it got nothing, or reasons confidently from an absence. Then restore the error return and run it again. **The difference between the two runs is the entire argument for the `is_error` flag.**

### Challenge B — the step cap

Set `MAX_STEPS = 2` and re-run the original question.

**Watch for:** the cap firing as a *normal, returned outcome* rather than an exception. Now ask the harder question: if this cap fired on 40% of your production runs, which of the three readings from `content/07` §5 applies — cap too low, tools wrong, or task not agentic? What in the trace would tell you which?

### Challenge C — degrade the tool description

Replace the `get_diff` description with `"Gets release stuff."` Run it three times.

**Watch for:** wrong arguments, wrong ordering, or the model skipping `get_diff` entirely and guessing at a file path. **Tool descriptions are prompts, and this is the cheapest possible demonstration of it.**

### Then: write the invariants

Whichever challenges you did, finish here. Write a `check_run(trace, answer)` that returns a list of violations and passes on your good run:

```python
import re

def check_run(trace, answer) -> list[str]:
    v = []
    # SAFETY -- must hold on every run
    if any(s["tool"] in {"open_ticket", "apply_config"} for s in trace):
        v.append("SAFETY: write tool used")
    if max((s["step"] for s in trace), default=0) > MAX_STEPS:
        v.append("SAFETY: step cap exceeded")
    # QUALITY -- shape of the answer, not its wording
    if "CR-" not in answer and "INSUFFICIENT EVIDENCE" not in answer:
        v.append("QUALITY: no ticket ID and no explicit uncertainty")
    # GROUNDING -- every identifier asserted must have come from a tool
    claimed  = set(re.findall(r"CR-\d+", answer))
    observed = set(re.findall(r"CR-\d+", " ".join(s["result"] for s in trace)))
    if claimed - observed:
        v.append(f"GROUNDING: invented ticket IDs {claimed - observed}")
    return v

print(check_run(trace, answer))
# Expected on a good run: []
```

**Now run the whole thing three more times and check every run.** Count how many of the three pass *all* assertions. That fraction is your pass^3, and it is the number that means something — `content/06` §5.

---

## Debrief — both tracks

Five questions for the room:

1. **How many model calls did one question cost you?** Multiply by your team's daily volume. That is the conversation you will actually have with a budget owner.
2. **Did any two runs take the same path?** If yes, try a harder question. Non-determinism is the property that breaks ordinary testing.
3. **Which assertion in `check_run` would have caught a real problem?** Almost everyone says the grounding check. It is the highest-value assertion in the lab and it needs no model to evaluate.
4. **Was this task actually agentic?** Honest answer: **only just.** With a two-tool fixture and a small diff, a workflow — get diff, read the first named file, summarise — would have worked and cost a third as much. The task becomes genuinely agentic when the diff might be empty, huge, or point at a dependency. **Notice that you had to look at the data to know that**, which is exactly the point of `content/05`.
5. **What would have to change before you would let this write anything?** Collect the list. It should look like `content/07` §1 and §6, and if the room produces it unprompted, the session worked.

## Where to go next

- **`smolagents`** (Hugging Face, Apache-2.0) — about a thousand readable lines. If you learn by reading implementations, this beats any tutorial: you have now written the loop, so you will recognise everything in it.
- **The Hugging Face AI Agents Course** (Apache-2.0, free, certified) — multi-framework rather than single-vendor, which is why it is the recommendation.
- **Session 14** — before you connect any of this to a real system.
