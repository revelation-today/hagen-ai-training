# Discussion & Poll Prompts — Session 1

For the 15-minute Q&A and in-session polls. Each prompt says what a good answer *surfaces* so the facilitator can steer rather than just collect opinions. The room is release / problem / configuration managers and developers — pull examples from their world (dashboards, incident reports, build systems, screening tools), not from AI research.

## The seed question (open the Q&A with this)

**"Where have you seen a system be confidently wrong? Was it *lying*, or *reconstructing*?"**

- *Surfaces:* the core reframe. Steer toward "reconstructing / pattern-completing." Lying needs a known truth to hide; these systems have none. Good answers name a system that produced a *plausible* wrong output that got trusted because it looked right (a mislabelled incident, a confident-but-wrong estimate, an autocomplete that finished the wrong sentence).
- *Facilitator move:* whatever example lands, apply the habit-of-mind question aloud: "was that checked against evidence, or trusted because it sounded right?"

## Live polls (A/B/C — show of hands or clicker)

**Poll 1 — "An LLM answering a factual question is basically a search engine." A) Agree B) Disagree C) It depends.**
- *Surfaces:* whether the "generation, not retrieval" model landed. Target answer: **Disagree** — it generates a probable continuation; sometimes that coincides with a fact, but there's no lookup underneath. "It depends" is a fair answer if someone notes tool-augmented models that *do* retrieve (foreshadow RAG, Session 13) — reward that nuance.

**Poll 2 — "A false memory feels different from a true one." A) True B) False.**
- *Surfaces:* the "certainty is not a truth signal" point. Target: **False.** If the room splits, that split *is* the lesson — you can't poll your way to the truth of a memory either.

**Poll 3 — "AI bias is a fundamentally different problem from hallucination." A) Agree B) Disagree.**
- *Surfaces:* the flagship synthesis. Target: **Disagree** — bias is pattern-completion from skewed data pointed at people; same mechanism, same counter-habit. Expect (and welcome) pushback that bias has a *moral* dimension hallucination lacks — both are true: same mechanism, different stakes.

## Discussion prompts (pick 3–4 as time allows)

| # | Prompt | What a good answer surfaces |
|---|---|---|
| 1 | Name a task in your job where you'd rather have an **auditable rule you can read** than a more accurate model you can't. Why? | The opacity cost from file 01; the release/config instinct for traceability. Root-cause work, change approval, compliance evidence all want readable rules. |
| 2 | Your model was trained on last year's incidents, and this year the system changed. What kind of failure are you now set up for? | Dependence on the examples (cost C2) + a preview of data drift (Session 13). The rules were inferred from a world that no longer exists. |
| 3 | If an LLM is right 99% of the time, is the human reviewer's job *easier* or *harder*? | The verification paradox (Session 13 proper): rarer errors are harder to catch, and a reviewer swimming in fluent correct output stops looking. Human-in-the-loop is necessary, not sufficient. |
| 4 | Where would **grounding** (giving the model a specific source document) help, and where would it *not*? | Intrinsic vs. extrinsic (Maynez et al. 2020): grounding mainly curbs *extrinsic* hallucination; a grounded model can still *contradict* its source (intrinsic). |
| 5 | Is "the model is objective because it just does maths" a safe thing to believe? | The laundering-of-skew point from file 03. Maths over skewed examples reproduces the skew and dresses it as neutrality. |
| 6 | Give an example where an LLM being a *pattern-matcher, not a search engine* is exactly what you want. | Balance — this isn't anti-AI. Drafting, rewording, summarising, brainstorming: tasks where fluent plausible language *is* the deliverable and truth is easy to verify or not the point. |

## Facilitation notes

- **Keep the voice skeptical, not cynical.** Every "AI can't be trusted for X" should be paired with an "and here's what it's genuinely good at." The point is calibrated use, not fear.
- **Protect the analogy caveat.** If someone argues "brains and LLMs work identically," agree the *parallel* is striking and restate that it's an analogy — the shared thing is *pattern-completion outrunning evidence*, not an identical mechanism.
- **Land on the habit, not a verdict.** The takeaway you want in the room's mouth on the way out is the question: *"checked against evidence, or trusted because it sounds right?"*
