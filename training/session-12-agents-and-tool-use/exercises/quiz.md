# Quiz — Session 12: Agents and Tool Use

Nine self-check questions. Answers at the bottom. Aim for seven; if you miss questions 2, 5, or 7, re-read `content/02`, `content/05`, and `content/06` respectively — those three carry the session.

---

### 1. Complete the definition in one sentence: an agent is ___.

### 2. A system runs three model calls in a fixed order — classify a ticket, look up a routing rule, draft a reply — with branching for four ticket types. Is it an agent? Justify with the test.

### 3. A ten-step agent uses a component that is correct 95% of the time at each step, with independent errors and no recovery. Roughly what end-to-end reliability should you expect, and what does that imply about model choice?

### 4. Name the six things in a hand-written ReAct loop that are not optional, and say what each one prevents.

### 5. Your colleague proposes an agent for "regenerating the weekly release dashboard." Give the verdict and the one-line reason.

### 6. Reflection: why does the criterion *"Is this summary accurate?"* buy you almost nothing, and what would you replace it with?

### 7. A vendor reports that their multi-agent architecture beats a single agent by 90% on their internal evaluation. State the single question that most efficiently tests the claim, and say why.

### 8. Your agent's step cap fires on 40% of runs. Name three possible causes and the one diagnostic that distinguishes them.

### 9. In one sentence, why is an agent the highest-risk deployment shape in this course — and which session handles it?

---
---

## Answer key

### 1. The definition

**An agent is an LLM wired to a loop and given the ability to act** — a semi-autonomous system that interacts with an environment, decides what to do next, and acts on a user's behalf. Three properties must all be present: **autonomy** (multiple steps without a check-in), **decision-making** (it chooses the action from options you gave it), and **adaptation** (what it does at step *n* depends on what it observed at step *n−1*). Miss any one and you have a prompt, a tool call, or a workflow.

The load-bearing distinction: **the model, not your code, decides what happens next.**

### 2. Ticket triage

**No. It is a workflow.** The test is *who wrote the control flow.* You did — the sequence of three calls exists in your repository and can be reviewed in a pull request. Branching for four ticket types does not change this; a ten-node graph with conditional edges is still a workflow if you drew the graph.

Applying the one-minute test: *can I draw the steps in advance?* Yes → workflow. *Does the number of steps depend on what it finds?* No → not an agent.

This is the better design, and calling it an agent would import risks it does not have while paying costs it does not need.

### 3. Compounding error

**0.95¹⁰ ≈ 60%.** A component you would reasonably call good — right nineteen times out of twenty — produces a system that fails two runs in five.

The implication for model choice: **the per-step error rate sits inside an exponent**, which is why agents typically need the most capable (and most expensive) models available. A one-point improvement in per-step reliability is worth far more in a ten-step loop than in a single call. It is also the best one-line argument for keeping runs short and putting checkpoints in.

*(The exact number is a simplification in both directions — a good agent can notice and correct an error, and a poisoned observation can also degrade every subsequent step. The shape of the curve is the point.)*

### 4. The six non-optional lines

| Line | Prevents |
|---|---|
| A **hard step cap** (`for step in range(...)`, not `while True`) | Infinite loops. The bill arrives before the alert does |
| **try/except around every tool call** | A raising tool crashing your process instead of becoming an observation the model can recover from |
| **`is_error: True`** on failed results | The model reading an error dict as data and reasoning confidently from it |
| **Matched `tool_use_id`** on every result | The API rejecting the turn |
| **Appending the full assistant content**, not just the text | Breaking the conversation structure by dropping the `tool_use` blocks |
| **A trace line per step** | Being unable to debug it, ever. Traces you did not capture do not exist |

### 5. The weekly dashboard

**Deterministic script. No LLM at all.**

Every step is known, the output is structured, and correctness is checkable. Adding a model converts a reliable report into an unreliable one and bills you per run. If someone wants prose commentary on top, that is *one* model call at the end, on data the script already computed — and a human reads it before it goes out.

This is the rule from `content/05`: if a deterministic script works, use the script. Not "consider" — use.

### 6. Reflection criteria

*"Is this summary accurate?"* reliably returns *"yes, with minor suggestions"* regardless of the draft, because it is not a criterion that can fail. You pay tokens for agreement, which is worse than nothing because it feels like verification.

Replace it with something checkable against the source: **"Does every claim in the summary appear verbatim in the source timeline? List any that do not."** That produces findings.

Two supporting points: a model critiquing itself shares its own blind spots, so ground the critique in the source material rather than in introspection — and better still, **use a real automated check.** The strongest reflection loops are not model critics at all; they are compilers, test suites, and schema validators.

### 7. The multi-agent claim

**"Was it compared at equal token budget?"**

It is the highest-yield single question because token spend is the dominant confound. In the best-documented public case, the same write-up that reported a large multi-agent win also reported that **token usage alone explained roughly 80% of the performance variance**, with the multi-agent system consuming about **15× the tokens** — and neutral follow-up work found the architectural advantage largely disappears when the thinking-token budget is held equal.

So the honest reading is that a good part of what looks like coordination winning is more tokens winning.

Three follow-ups if you have time: **cost per task** (essentially no major agent benchmark scores it, so 88% at $50/task ranks identically to 88% at $0.50/task); **same harness?** (scores swing with the scaffold before the model does any work); and **pass^k or pass^1?** (pass^k commonly runs 15–25 points lower, so a 90% benchmark score is roughly 70% production reliability).

### 8. The step cap firing at 40%

Three causes:

1. **The cap is too low** for a genuinely long task.
2. **The tools are wrong** — the agent lacks a tool it needs, or two overlapping tools make it thrash between near-identical options.
3. **The task was never agentic** — there is no data-dependent structure to discover, so the model wanders.

**The diagnostic is the trace.** Look at whether the capped runs are long-and-productive or long-and-repetitive. If a tool is being called repeatedly with identical or near-identical arguments, it is (2) or (3), not (1) — and raising the cap will make the bill worse without making the answers better. Cap-fire rate belongs on your dashboard from day one for exactly this reason.

### 9. Why agents are the highest-risk shape

**Because an agent is precisely "an API acting on model output"** — the hazard initiating mechanism the safety framework this course inherited names by name. It reads content it did not author (some of which an outsider can influence), it holds credentials and tools, it acts without a human between the decision and the consequence, and its loop feeds observations back into its own context so a single poisoned observation influences every subsequent step.

**Session 14** handles it: prompt injection direct and indirect, the three-precondition test, and excessive agency. Read it before connecting an agent to anything you care about — not after.
