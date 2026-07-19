# Key Takeaways — Session 11

---

## Part A — Prompting II

- **Every production prompt has six layers:** role, context, delimited input, precise task, constraints, output contract. Stable layers first, variable input last — it caches better and it diffs better.
- **The two highest-leverage lines you can add** are naming the specific things it must not invent, and giving it an explicit escape hatch for uncertainty. Both address the same root cause: a model completing a pattern will fill a slot whether or not it has the information.
- **"Insufficient information" must be a first-class outcome**, with a deterministic aggregation rule that turns it into a `HOLD`. Otherwise honest uncertainty gets rounded up to a pass.
- **Diagnose before you treat.** Five categories: missing context, ambiguous task, unconstrained output, capability/model mismatch, wrong approach entirely. Each has a different fix and only one of them is "reword it".
- **One change per pass, same input every pass.** Change two things and you learn nothing about either.
- **Escalate in order:** context → procedure → output contract → examples → model/reasoning budget → decomposition → stop. Each step costs more than the last.
- **Category 5 — "this is not a prompting problem" — is the one people skip**, and the most expensive to skip.
- **A prompt without a test set is a rumour that happened to work once.** Twenty cases, pass/fail assertions on *properties* not exact output, versioned with the prompt.
- **Grade in tiers:** rules (free, deterministic) → model-as-judge (cheap, noisy, itself an unvalidated prompt — calibrate it against human labels) → human (expensive, authoritative). Push everything as far left as it will go.
- **The suite is the instrument that settles every "which one?" question** in this session — prompt version, model choice, thinking budget. All three, same method, twenty minutes each.
- **A better prompt often beats a bigger model, at a tenth of the cost.** You will not believe this until you measure it, which is the point.
- **When the model version changes, re-run the suite before trusting anything.** This alone repays the afternoon it costs to build.

## Part B — Working With Claude

- **⚠️ Every product-specific claim here needs verification against current Claude documentation at delivery.** Principles are durable; feature names are not.
- **Surface choice matters more than phrasing.** Three questions decide it: how often will you do this, is there stable context you keep re-pasting, and does a human read every output before it matters?
- **Re-pasting the same background is the clearest signal you are in the wrong surface** — and the drift it causes is what people misdiagnose as the model being "inconsistent".
- **Project context is a dependency and it rots.** Date it, own it, review it, keep it small. When output degrades, suspect the context first — contradictions between attached documents produce apparent randomness.
- **Ask for the artifact, not advice about the artifact.** Concrete output generates specific criticism; abstract advice generates agreement.
- **Extended thinking is a cost/latency dial, not a quality setting.** It fixes exactly one diagnosis category — capability mismatch. It does not add knowledge, fix missing context, or prevent hallucination.
- **Stop paying prompt space for "think step by step".** Ask instead for a specific inspectable intermediate — the durable version of the technique.
- **MCP is a real standard**, foundation-governed, multi-vendor. Host → client → server, stdio or Streamable HTTP (HTTP+SSE deprecated), stateless at the protocol layer. Tools act; resources are read.
- **The server is the enforcement point, never the model's judgement.** Least privilege in code, not in a system prompt.
- **Most connector ideas should be a paste.** Build one when repetition, volume, or volatility forces it — read-only first, owned, and security-reviewed.
- **A connector plus untrusted content in context plus a tool that can act is the prompt-injection configuration.** Session 14.
- **Feed failures back.** Failure → scratchpad → test case → prompt fix → suite re-run → promote recurring context into the Project. Without that loop you accumulate anecdotes instead of capability.
- **Sanitise by default.** The risk usually arrives inside a paste, not inside a question.

---

## The honest limitations, restated

Because a session that only sells its methods is not this course's voice:

- Twenty cases is a smoke test, not a characterisation of behaviour.
- Temperature 0 reduces variance; it does not deliver determinism.
- A model-as-judge is a model, with every failure mode Session 1 described.
- Pass rate measures the properties you thought to assert. A prompt can score 20/20 and be unhelpful.
- A test suite nobody owns rots into false assurance, which is worse than none.
- Extended thinking's visible reasoning is generated text about the process, not a log of it.
- A well-formatted artifact actively suppresses scrutiny. That is its most dangerous property.

---

## If you remember one thing

> **A prompt with a test set is an engineering artifact; a prompt without one is folklore. Everything else in this session — surface choice, thinking budget, model choice, connectors — is a question you now have an instrument to answer, instead of an opinion to defend.**

---

## Where this goes next

| Session | Connection |
|---|---|
| **12 — When AI Is Confidently Wrong** | Why the verification habits in `08` are not optional, and why a 99%-correct system is *harder* to supervise than a 90% one |
| **13 — Security, Privacy, and Mitigation** | The prompt-injection surface that `07` flagged and deliberately did not open. Read it before you build a connector, not after |
| **14 — Roles** | What all of this means for release, problem, and configuration management as jobs |
