# Key Takeaways

---

## Half A — capability and its ceiling

- **LLMs excel at one shape of task:** language in, language out, where the information is already in the input and a human can cheaply check the result. Transformation, drafting, compression, pattern-spotting. All four are genuinely valuable and all four require the human to absorb the verification cost.
- **Three failures are structural, not temporary.** Novel reasoning (extrapolation beyond the trained distribution), guaranteed correctness (the output is a sample, never a proof), and ground truth (nothing in the mechanism touches reality). These follow from "predict the next token" and are not fixed by scale.
- **The tell for a structural limit:** if your sentence needs *always*, *never*, *all*, or *exactly*, an LLM cannot be the thing that makes it true. Use a checker.
- **The S-curve:** skill coverage saturates well short of 100% while cost per additional skill rises steeply. **It is not AI capability that is exponential — it is the expense of producing it.**
- **The S-curve was wrong about the height of the ceiling and right about the shape of the cost.** Capability rose further than pessimists expected; last-mile economics behaved exactly as predicted.
- **The proof-of-concept-to-production gap is where the work is.** Representative data, failure catalogues, monitoring, drift detection, rollback, change approval, accountability, real-volume cost. Read that list again and notice it is a set of job descriptions.
- **The gap does not close.** It is downstream of the S-curve, it grows with deployment rather than shrinking with capability, and accountability cannot be automated even in principle.

## Half B — the jobs

- **"Replaced or safe" is the wrong question.** Jobs are bundles of tasks. Ask which sub-tasks get **automated**, which get **augmented**, and which get **harder**.
- **The fourth bucket is the story.** Work gets harder for three reasons: everyone else's output is now AI-shaped (fluency decoupled from competence — you lost a career-long heuristic); the verification paradox (the better it gets, the worse you get at catching it); and volume rising without matching verification capacity.
- **Per role, in one line each:**

| Role | Automated | Stays human | The thing that gets harder |
|---|---|---|---|
| **Release manager** | Notes, rewrites, translation | Go/no-go, defect acceptability, negotiation, rollback | Verifying usually-correct notes; more change, same gate |
| **Problem manager** | Timelines, summaries, correlation | Root cause, which anomaly matters, blameless facilitation | Anchoring on a fluent wrong cause |
| **Configuration manager** | Diff summarisation (drift detection: use a *deterministic* tool) | CMDB truth, approval, remediation ownership | Plausible-and-wrong AI config that passes validation |
| **Developer** | Boilerplate, scaffolding, translation | Design, security judgement, accountability | Reviewing much more code that is usually fine |

- **The decision rule:** who answers if it's wrong? → can you verify it cheaply? → do you need a guarantee? Three questions, and a surprising number of them correctly resolve to "use a deterministic checker, not AI."
- **Never delegate:** the decision to ship, the approval, the root cause, the record-versus-reality reconciliation, any assertion of correctness or security, and accountability in any form.
- **The honest caveats:** the argument depends on verification being staffed; "composition first" is a sequence, not a guarantee; the junior→senior pipeline problem is real and unsolved; and some of this displacement was overdue deterministic automation that AI budgets finally funded.
- **The technology does not decide.** Whether verification gets staffed or thinned is a management choice, influenced by how well the people in these roles can articulate what their judgement catches. Which is why measuring and writing that down is a concrete, defensive act.

---

## If you remember one thing

> **AI changes the composition of these jobs before it eliminates any of them — the human moves up the stack toward judgement, verification and accountability, which is exactly where release, problem, and configuration management already live. Your exposure is not to being replaced; it is to being handed the verification load without the time to do it. Ask for the time, in writing, before the incident.**
