# Key Takeaways — Session 12

---

## What an agent is

- **An agent is an LLM wired to a loop and given the ability to act.** Not a smarter model, not a better prompt — a control-flow decision.
- Three properties, all required: **autonomy** (multiple steps without you), **decision-making** (it chooses the action), **adaptation** (step *n* depends on what step *n−1* observed). Miss one and it is a prompt, a tool call, or a workflow.
- The defining change: **the model, not your code, decides what happens next.**
- Your code still executes every tool. The model only emits a request against a schema. That property is the whole security posture.
- The one-minute test on any product description: *can I draw the steps in advance?* (workflow) *does the number of steps depend on what it finds?* (agent).

## Workflows vs. agents

- **Workflow = predefined code paths you wrote. Agent = the model directs its own process.** The test is who wrote the control flow.
- Every operational property — cost, latency, debuggability, testability, reviewability, blast radius — favours the workflow. Agents win **one** row: data-dependent step sequences.
- Four gates, in order: **complexity · value · viability · cost of error.** One "no" and you step down a rung.
- **Most real systems should be a workflow with one agentic node.** Chaining → routing → parallelism → orchestrator-workers → free-running loop, one increment at a time, measuring each.
- A ten-node graph with conditional edges is still a workflow if you drew the graph. That is a compliment.

## The loop itself

- **ReAct = Thought → Action → Observation, repeated.** Interleaving is the mechanism: reasoning grounded in what the last action actually returned.
- **Reading a trace is the transferable skill.** Watch input tokens grow every step, watch step *n* depend on step *n−1*, and treat the Thought as narration rather than a log of the model's internals.
- Six non-optional lines: **step cap · try/except around every tool · `is_error` on failures · matched `tool_use_id` · append the full assistant content · trace every step.**
- **Tool descriptions are prompts.** Encode procedure in them, keep schemas strict, keep the tool count low, keep results small — every observation is re-sent on every later step.
- Frameworks wrap this loop and genuinely disagree about the right abstraction. **Learn the loop; treat frameworks as replaceable.**

## Plan & Execute, and Reflection

- **Plan & Execute:** one expensive planning call, N cheap execution calls, and a **re-plan edge you must not omit** — a plan written before any observation is a plan written in ignorance. Roughly 3× cheaper than a large-model loop. Always first measure whether the cheap model can do the whole job alone.
- **Reflection:** generate → critique against **explicit, failable** criteria → revise, capped at 2–3 rounds. "Is this good?" returns "yes, with minor suggestions" every time.
- A model critiquing itself shares its own blind spots. Ground the critique in source material — or better, **in a real automated check.** The strongest reflection loop is not a model critic at all; it is a compiler, a test suite, or a schema validator.
- **Every pattern you stack multiplies calls**, and each call carries the whole trace.

## When not to build one

- **If a deterministic script works, use the script.** Not "consider" — use.
- **The default answer is no.** Agents earn their place only when the *step sequence itself* is data-dependent, and that is a narrow class.
- **Read-only before acting. Gated before autonomous. Measured before trusted.**
- Agent failures in production are overwhelmingly **engineering** failures: the task was never agentic, unbounded loops, no trace, silent tool errors, no agreed stop condition. Only compounding error involves the model at all.
- **Mistakes compound.** 95% per step over ten steps is **60%** end to end. Nothing in the architecture fixes that; the architecture creates it.

## Reading the evidence

- **Two major labs published flatly opposing multi-agent advice within days. One later reversed. Neither retracted.**
- The headline **+90% multi-agent win** sits in a write-up that also reports **token usage explained ~80% of the variance**, at **~15× the tokens.** Read the caveat paragraph — good work contains one, bad work does not.
- **Neutral, equal-token-budget research finds the architectural advantage largely evaporates.**
- The two best-supported claims in the whole literature are the ones both camps reached independently: **start simple**, and **context accumulates, focus degrades.**
- **The four questions:** equal token budget? cost per task? same harness? pass^k or pass^1?
- **A 90% benchmark score is roughly 70% production reliability.** pass^k commonly runs 15–25 points below pass^1.
- **Score cost and quality in the same table.** Essentially no major agent benchmark does — so 88% at $50/task ranks identically to 88% at $0.50/task.

## Production

- **Bound three things:** the tools (read-only first, least privilege in code), the loop (step, token, time, per-tool caps — all four), and the consequences (staging artifacts, reversibility, logged action-to-trace linkage).
- **Trace from day one.** Steps per run (median and p95), tool error rate, cap-fire rate per cap, cost per successful task.
- **Prompt caching is the biggest cost lever for agents**, because the stable prefix is re-sent on every step.
- **Test properties, not trajectories.** Safety invariants, grounding checks that verify every asserted identifier came from a tool result, twenty real tasks, k runs each, pass^k reported.
- **The human gate must be qualified, blocking, and checkable** — and the better the agent gets, the harder that is to staff. Design the output to be reviewable: small diffs, cited evidence, an explicit list of what was *not* verified.

---

## The honest limitations, restated

Because a session that only sells its methods is not this course's voice:

- Twenty tasks is a smoke test, not a characterisation of behaviour.
- Temperature 0 reduces variance; it does not deliver determinism, and it does nothing about the tools' own changing data.
- The visible "Thought" is generated text about the process, not a record of it.
- A grounding check verifies the identifiers you thought to check for.
- A model-as-judge is a model, with every failure mode Session 1 described.
- The cost multipliers in these files are ratios from one worked scenario, not prices. Recompute them.
- The multi-agent evidence does not settle the question. Holding "unsettled" as a position is the accurate stance, not a cop-out.

---

## If you remember one thing

> **An agent is an LLM in a loop with the ability to act — and that one architectural change buys you data-dependent task shapes at the price of determinism, predictability, an order of magnitude in tokens, and a much larger blast radius. Most of the time the right answer is a workflow. When it is not, bound it, trace it, cost it, and gate it.**

---

## Where this goes next

| Session | Connection |
|---|---|
| **13 — Risk I: When AI Is Confidently Wrong** | Why the human gate in `07` is harder to staff than it looks: the better the agent gets, the harder its rare errors are to catch. The verification paradox applied to a system that acts. |
| **14 — Risk II: Security, Privacy & Mitigation** | **The hand-off this session sets up deliberately.** An agent is precisely *"an API acting on model output"* — the initiating mechanism the safety framework names. Prompt injection, indirect injection through the content an agent reads, the three-precondition test, and excessive agency. **Read it before you connect an agent to anything you care about.** |
| **15 — What AI Can and Can't Do** | Where agent capability actually sits: large capability gains, reliability still lagging capability, and what that means for which work is automatable this year rather than eventually. |
