# Session 13 — Risk I: When AI Is Confidently Wrong

**Block:** Risk it · **Goal covered:** 7 (first of two) · **Format:** 45 min content + 15 min Q&A

---

## One-paragraph summary

This is the session that teaches you to *not be fooled by a number*. It has three moving parts. First, hallucination revisited — not as a curiosity but as a thing you have to engineer around, with the two mitigations that actually work (ground the model in retrievable source material; make a qualified human verify) and the counter-intuitive truth that sits behind both: **the better the model gets, the harder its rare errors are to catch. If it's right 99% of the time, spotting the 1% is harder, not easier.** Second, why your metric lies — a model that predicts only people named Michael will quit their job is wrong on every count that matters and still reports **98% accuracy**; we work the full confusion matrix and watch precision, recall and F1 fall apart while accuracy sails on. Third, the centrepiece: a **vendor role-play**, run as a staged reveal. A vendor claims 99%. You ask the right question and get 79.8%. Then you bring in one outside fact — the base rate — and the number collapses to **3.39%**. From "buy it" to "don't buy it" in three questions. The session closes on **p-hacking**, framed honestly as *not usually malicious — human nature under pressure and career survival*, and on the rule that human-in-the-loop is **necessary and not sufficient**.

For a room of release, problem and configuration managers, this session inverts an intuition they carry into every vendor meeting: *more accuracy = safer*. It doesn't. It moves the burden of proof, and it moves it onto you.

## Audience & level

Qualcomm release / problem / configuration managers and developers. Some prior AI exposure assumed. The statistics are worked from scratch — no prior probability background is needed beyond "what a percentage is." Developers get a short Python lab (scikit-learn confusion matrix + a Bayes calculator); managers get the vendor role-play, which is the part they will use first and use most. **Nobody needs to run code to get full value from this session.**

This is the session the course proposal recommends as the **pilot**: it is the strongest material in the corpus, needs no lab environment, and is directly about a task this audience performs — evaluating a vendor's AI claim.

## Learning objectives

After this session a participant can:

1. **Explain** why grounding/RAG and human verification reduce hallucination risk without eliminating it, and name which class of hallucination each addresses.
2. **Argue** the 99%/1% inversion — that rising model accuracy makes residual errors *harder* to catch, not easier — and state what that implies for staffing a human-in-the-loop control.
3. **Compute and interpret** a confusion matrix: sensitivity, specificity, precision, negative predictive value, accuracy, F1 — and say which one a class-imbalanced problem makes worthless.
4. **Apply** Bayes' theorem to convert a vendor's quoted sensitivity into the precision they will actually see in *their* population, given a base rate.
5. **Run** the four-question vendor interrogation — *What exactly is that 99%? What's the precision? What was the base rate in your test sample? What's the base rate in my population?* — and reach a defensible buy/don't-buy verdict.
6. **Recognise** p-hacking in its non-malicious form, name the three pressures that produce it, and name one control that catches it.
7. **Decide** whether a proposed human-in-the-loop control is real or theatre, by asking whether the human is *equipped* to catch the error.

## Prerequisites

- **Session 1** — hallucination as pattern-completion outrunning evidence; intrinsic vs. extrinsic hallucination.
- **Session 8** — the confusion matrix was introduced in the hands-on lab. This session assumes you have *seen* one; it re-derives everything, so a gap is survivable.
- Helpful but not required: Session 2 (vocabulary), Session 9 (how LLMs work).

Session 13 sets up **Session 14** (Risk II: security, privacy, mitigation in practice) and feeds **Session 15** (capability limits, the proof-of-concept-to-production gap).

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook — plant the claim** | Put the vendor's slide on screen: *"Our AI identifies 99% of at-risk cases."* Ask for a show of hands: buy, don't buy, need more info. Do **not** resolve it. Say: "we'll come back to this at minute 28." |
| 3–10 min | **Hallucination revisited, with mitigations** | Why it happens (extrapolation into sparse space); intrinsic vs. extrinsic; grounding/RAG and what it fixes; verification as the other lever; the verification paradox. |
| 10–15 min | **The 99% trap** | Automation complacency and the startle factor. Human-in-the-loop is necessary and not sufficient. Is your human *equipped*? |
| 15–23 min | **Why your metric lies** | The Michael parable. Full confusion matrix worked live: sensitivity 0, precision 0, F1 undefined, accuracy .98. The degenerate 99% model. |
| 23–28 min | **Base rates — the warm-up case** | 85% of homicidal offenders played violent video games → P(homicidal \| gamer) = 0.02%. The nested-set diagram. The rule: never associate a common trait with an uncommon one. |
| 28–39 min | **The vendor role-play (centrepiece)** | Staged reveal: 99% → *what kind of 99%?* → 79.8% → the outside fact → **3.39%** → verdict. Then the correction, and the AI-tooling swap. |
| 39–44 min | **P-hacking** | Six techniques, three pressures, one honest framing. What a validation set is actually for. |
| 44–45 min | **Close** | The four questions, on one slide, to photograph. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. Seed question: *"Where in our own reporting do we quote a number that is technically true and practically misleading?"* |

**Is 45 minutes honest?** It is tight but it works, *provided you do not teach Bayes' theorem as mathematics*. Teach it as counting people. Every number in this session can be reached by counting bodies in a table of 100,000 — do it that way and the room stays with you. If you run long, the first cut is the p-hacking segment (drop to 3 minutes: the three pressures and the Coase idea, skip the six techniques); the second cut is the video-games case (it is the warm-up for the vendor reveal, not the reveal itself — but the reveal lands harder if the room has seen the pattern once already, so cut it only under real pressure). **Never cut the vendor role-play.** It is the session.

## Materials & tools

- Slides: `slides/outline.md` → deck built per `../powerpoint_instructions.md`. Build the vendor scenario as a genuine **staged reveal** (one number per slide click); it dies if the room can read 3.39% while you are still saying 99%.
- Optional live demo: an LLM confidently fabricating a citation, a changelog entry, or a person's biography. Run it on the presenter's own sandbox account. Do **not** put a real colleague's name into a public model.
- `exercises/lab.md` — a ~25-minute Python lab (scikit-learn confusion matrix; a Bayes calculator; a prevalence sweep). Colab-first, JupyterLite fallback. Runs offline after `pip install scikit-learn`.
- A printed one-pager of the four vendor questions is worth handing out. People take it to meetings.

## Source & licence note

| Source | Role in this session | Reuse verdict |
|---|---|---|
| **The statistics and Bayes arithmetic in this session** | Authored for this course — every number re-derived and checked | **Ours** — no third-party constraint |
| **scikit-learn** (documentation, API, metric definitions) | The confusion-matrix code and metric formulas | **SLIDE-SAFE** (BSD-3 — attribute) |
| **Maynez et al. 2020**, *On Faithfulness and Factuality in Abstractive Summarization* (ACL Anthology) | Intrinsic vs. extrinsic hallucination definitions, on slides | **SLIDE-SAFE** (CC BY 4.0 — attribute) |
| **RAGAS** (Es et al., EACL 2024) and **ARES** (Saad-Falcon et al., NAACL 2024), via ACL Anthology | Groundedness / faithfulness evaluation — that "is it grounded?" is itself a measurable, automatable property | **SLIDE-SAFE** (CC BY 4.0 — attribute) |
| **Ioannidis 2005**, *Why Most Published Research Findings Are False* (PLoS Medicine) | The p-hacking / replication-crisis anchor | **SLIDE-SAFE** (CC BY — verify at delivery); safest to cite rather than quote |
| **Deep Learning for Beginners — Day 3** (Nield, O'Reilly), §Validation Pitfalls | Origin of the Michael parable, the vendor scenario, the video-games case, the p-hacking framing | **LINK-ONLY** — structure and ideas rebuilt in our own words and numbers; never reproduce slides |
| **LLM System Safety and Security** (Nield, O'Reilly) | The 99%/1% startle-factor trap; human-in-the-loop necessary-not-sufficient; the verification paradox | **LINK-ONLY** — paraphrase, attribute the framing |
| Coase ("torture the data long enough…"), Ng, Chollet | Pull quotes | **LINK-ONLY** — **paraphrase**; do not put the quotation on a slide |

**Corrections carried in this session** (house rule: never present a source-deck error as fact). The source deck's vendor arithmetic and its video-games follow-up both contain slips. We teach the reveal as the source stages it — because the *staging* is the asset — and then correct the arithmetic openly, in front of the room. That correction is not a footnote; it is one of the best two minutes in the session, because the source deck's own error is a *milder version of the exact fallacy it is exposing*. Details in `content/05` §"Correcting the source" and `content/04` §"Correcting the source". Full provenance: `resources/sources.md`.
