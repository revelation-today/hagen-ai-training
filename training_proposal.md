# Proposal: AI Training Series
### Internal training — Qualcomm (Release / Problem / Configuration Management + Developers) · 16 sessions · 45 min + 15 min Q&A

*Revised against the full brief in `instructions/proposal_training.md` (11 stated goals). Supersedes the earlier 13-session draft.*

---

## 1. The Brief, As Stated

| | |
|---|---|
| **Audience** | Qualcomm team — release, problem, and configuration management, plus developers. Some prior exposure to AI. Technically literate; hands-on coding is in scope. |
| **Format** | 45 min content + 15 min Q&A per session. |
| **Language** | English. |
| **Depth** | Cover all 11 goals at survey depth; **deepen three** — prompting, risk & mitigation, and hands-on deep learning (two sessions each). |
| **Hands-on level** | **Keras, not NumPy-from-scratch.** Build and train a real network in a few lines, understand each line, skip the calculus. |
| **Quantum** | A short closing outlook segment, not a full session. |

**The 11 goals, and where each is covered:**

| # | Goal (from the brief) | Session(s) |
|---|---|---|
| 1 | What is AI & how it relates to human thinking (learning by example; hallucination vs. prejudice) | **1** |
| 2 | Key terms (AI, ML, LLM, token, deep learning…) + the cost of AI (tokens) | **2** |
| 3 | Methods explained (deep learning, random forest, LLM, unsupervised learning) | **3, 4, 5, 6, 9** |
| 4 | Hands-on deep-learning programming | **7, 8** *(deepened)* |
| 5 | Practical tips for writing great prompts | **10, 11** *(deepened)* |
| 6 | Practical hints for working with Claude (scratchpad, …) | **11** |
| 7 | Understanding risks & mitigation | **12, 13** *(deepened)* |
| 8 | What AI can and cannot do | **14** |
| 9 | Will AI take our jobs (release/problem/config manager, developer)? | **14** |
| 10 | What is AGI | **15** |
| 11 | Outlook: impact of quantum computers | **15** (closing segment) |

Every goal has a home; the three deepened goals get two sessions each.

---

## 2. Two Things You Should Decide With Eyes Open

**Source exclusion — the Cisco deck.** One of the seven source decks ("Mastering the Fundamentals of AI and ML") is stamped `Cisco Confidential` on every slide and authored by Cisco employees. Using another company's confidentiality-marked internal material to train Qualcomm employees is not defensible, so it is **excluded**. It was the *only* source for three of your required method topics — **unsupervised learning, random forests, and transformer internals** (goals 3). Those sessions (4, 5, and parts of 9) are therefore rebuilt from clean, license-checked public material, noted per session. This is authoring work, not a hole — the topics are textbook-standard and well served by openly-licensed sources.

**Four goals have no source material at all** and must be written from scratch:
- **Goal 6 — working with Claude** (scratchpad, projects, etc.): nothing in the corpus.
- **Goal 9 — will AI take *our* jobs** (release/problem/config management specifically): nothing.
- **Goal 11 — quantum outlook**: nothing, and inherently speculative.
- **Goal 2 — token *costs***: only scattered asides.

Roughly **half this series is authored**, concentrated in exactly the areas your (mostly 2024-era) source decks don't reach: modern prompting, RAG, security, agents, the Qualcomm-role job analysis, Claude workflow, and quantum. Set expectations accordingly — this is a curriculum built *using* the decks as one input, not a re-cut of them.

---

## 3. The Spine

The brief is a survey, so the series is sequenced as a learning journey rather than a single argument:

> **Understand it** (1–2: what AI is, the vocabulary) → **know the methods** (3–6: the four families) → **do it** (7–9: build a network, then see how LLMs work) → **use it well** (10–11: prompting, Claude) → **use it safely** (12–13: risk) → **judge it** (14–15: limits, jobs, AGI, quantum).

One editorial choice worth flagging: three of your source decks share a distinctive, skeptical voice — *AI capability is systematically over-claimed, and the interesting part is the gap between the demo and reality.* For a room of release, problem, and configuration managers — whose entire discipline is "what happens when the demo meets production" — that voice is a gift. The series leans into it. It is honest without being cynical, and it makes the "will it take my job" conversation (Session 15) credible rather than reassuring-noise.

---

## 4. The Sessions

### Session 1 — What AI Is, and How It Relates to Human Thinking
**Goal 1.** The opener. No jargon yet — build the core analogy and the core disanalogy.

- **Learning by example, not by rules** — the inversion at the heart of ML: traditional software is *data + rules → answer*; machine learning is *data + answers → rules*. This one slide reframes everything that follows.
- **Hallucination vs. prejudice — the sharpest idea in the brief, and worth the whole session.** Human memory is not a recording; we *reconstruct* it, and reconstruction produces confident, coherent errors — false memories, filled-in gaps, bias. An LLM's hallucination is the machine analogue: it doesn't retrieve a fact, it generates a plausible continuation, and sometimes plausibility and truth diverge. *Prejudice* is the same mechanism pointed at people — a pattern over-generalised from skewed training data. Framing hallucination and human prejudice as the *same failure mode* — pattern-completion outrunning evidence — is genuinely illuminating and nothing in the corpus does it directly.
- **"An LLM is autocomplete on steroids — a pattern-matching engine, not a search engine looking up facts."** The single most useful mental model in the series; everything about cost, risk, and capability follows from it.

**Source:** AGI deck (reconstructive memory ↔ hallucination); LLM Safety deck (autocomplete framing). **Authoring:** the hallucination-vs-prejudice synthesis is yours to write.
**Q&A seed:** Where have you seen a system be confidently wrong? Was it lying, or reconstructing?

---

### Session 2 — The Vocabulary, and the Cost Meter
**Goal 2.** The reference session everyone comes back to. Two halves: the words, and the bill.

- **The nested vocabulary, built once, cleanly:** AI ⊃ Machine Learning ⊃ Deep Learning ⊃ LLMs. Plus the terms the brief names — **token**, model, training vs. inference, parameters vs. hyperparameters — each defined against the one running example.
- **The token as the unit of everything.** A token ≈ ¾ of a word in English. It is simultaneously how the model *reads*, how it *generates*, and **how you are billed**. Live demo: the OpenAI tokenizer (`platform.openai.com/tokenizer`) — paste a sentence, watch it fragment; paste code or German, watch the count jump.
- **What AI actually costs (authored — the brief asks for this and no deck covers it):** input tokens vs. output tokens, priced separately; the context window as a cost multiplier (every turn re-sends the whole conversation); why RAG, long documents, and agents multiply spend; prompt caching as the main lever. A worked example: the same task at three model tiers, with the real 2026 price difference. **The point for this audience:** cost scales with tokens, not with "requests" — which is counter-intuitive to anyone from a per-transaction billing mental model.

**Source:** LLM Safety deck (LLM mechanics); tokenizer tool. **Authoring:** the entire cost half. **Currency:** pricing must be pulled fresh at delivery.

---

### Session 3 — Methods I: Learning From Data
**Goal 3 (foundation).** Before the four named methods, the shared machinery — so 4, 5, and 6 don't each re-explain it.

- Supervised learning: labels, features, the train/validation/test split (70/15/15) and *why* you hold data back.
- What "a model" is (the dark-clouds-mean-rain intuition), and what training vs. inference cost.
- Regression vs. classification; how a probability becomes a decision.
- **The decision heuristic this audience will actually use:** structured/tabular data → simple models; perceptual/fuzzy problems → neural networks. And the discipline note: *use the simplest model that works; neural networks are expensive and opaque.*

**Source:** DL Day 1 §I (taxonomy, when-not-to); the supervised-learning framing. **Authoring:** light.

---

### Session 4 — Methods II: Unsupervised Learning
**Goal 3.** Finding structure with no labels. ⚠️ **Rebuilt from public sources** (this topic existed only in the excluded Cisco deck).

- Clustering: **K-means** (the animated centroids → assign → recentre → repeat loop; the elbow method for choosing K), and **DBSCAN** (density-based; finds odd shapes; separates noise).
- Dimensionality reduction: **PCA** (find the directions that matter) and **t-SNE / UMAP** (for visualising high-dimensional data). Rule of thumb: *t-SNE to see, PCA to compute.*
- **The application that lands for this audience:** anomaly detection. A cluster model that groups "normal" configurations or incidents and flags the outlier is a problem/config-management tool, not an abstraction.

**Source:** ❌ Cisco-excluded. **Rebuild from:** scikit-learn's user guide and examples gallery (BSD-licensed — genuinely reuse-safe), StatQuest videos, r2d3.us's visual intro. **Authoring:** full — but the material is standard and well-supported. *License note: prefer scikit-learn's own figures/text (BSD) over all-rights-reserved blog explainers for anything you put on a slide.*

---

### Session 5 — Methods III: Decision Trees and Random Forests
**Goal 3.** The most *interpretable* ML method — a strong contrast to the black-box LLM. ⚠️ **Rebuilt from public sources** (Cisco-only in the corpus).

- A decision tree as a flowchart the machine learns; the "buys a computer?" worked example.
- **Gini impurity** — how the tree picks each split — framed as the same "cost/distance" idea seen in every other method.
- Why one tree overfits, and how a **random forest** fixes it: bootstrap, bagging, out-of-bag error, majority vote.
- **Why this method matters to this room:** it *shows its reasoning*. For release/problem/config work, an auditable model you can read is often worth more than a more accurate one you can't. This is the session that makes "explainability" concrete.

**Source:** ❌ Cisco-excluded. **Rebuild from:** scikit-learn (BSD), StatQuest. **Authoring:** full; standard material.

---

### Session 6 — Methods IV: Deep Learning, Conceptually
**Goal 3.** How a neural network actually works — the ideas, so Sessions 7–8 can be hands-on without hand-waving.

- A neuron is a weighted sum plus a nonlinearity; a network is layers of these; "deep" just means more than one hidden layer.
- The running example: predict light-vs-dark text for any background colour (RGB in, one probability out).
- Forward propagation as "push the numbers through"; **training as adjusting the weights to reduce error** — gradient descent and backpropagation *by intuition* (the flashlight-in-the-mountains metaphor), **not by calculus.** Per your steer, the math is shown, not derived.
- Activation functions (why nonlinearity matters at all), overfitting, why we hold out test data.

**Source:** DL Days 1–2 (conceptual layer only — the from-scratch NumPy is deliberately dropped per your Keras-level answer). **Authoring:** re-pitch from "build it" to "understand it."

---

### Session 7 — Hands-On I: Build and Train a Network in Keras *(deepened)*
**Goal 4.** First of two hands-on sessions. Everyone leaves having trained a model.

- Live in a Colab notebook: load the colour dataset, build a 3→3→1 network in ~5 lines of Keras, compile, `fit()`, watch accuracy climb.
- Each line explained — `Dense`, `relu`, `sigmoid`, `epochs`, `batch_size`, the `/255` scaling — but no derivations.
- **The honest moment:** run it once with random weights (chance accuracy), then train it, so the audience *sees* learning happen rather than being told about it.
- Type-along, not watch-along. The proven "hold one code block, change one thing, re-run" rhythm from the source course.

**Source:** DL Day 1 (the TensorFlow/Keras build, p.38/40). **Authoring:** the notebook and its narration — the source's exercise slides are title-only, so the lab is written from scratch. **Dataset:** `tinyurl.com/y2qmhfsr` — ⚠️ *verify it still resolves before delivery (2024 short-link; verification pending session-limit reset).* If dead, the identical dataset is trivial to regenerate or swap for scikit-learn's built-ins.

---

### Session 8 — Hands-On II: Make It Better *(deepened)*
**Goal 4, second half.** From "it trains" to "it's actually good" — the part that maps onto real engineering.

- Overfitting made visible: train accuracy high, test accuracy low — and what to do (more data, dropout, early stopping).
- Tuning the knobs that matter: learning rate, epochs, network size — and watching each one help or hurt.
- **Reading a confusion matrix**, precision/recall — how to tell whether a model is actually working, not just reporting a nice number. (Directly sets up Session 13's "your metric is lying.")
- A second dataset the room chooses, to prove the workflow transfers.

**Source:** DL Day 3 §Testing & Validation (confusion matrix, overfitting). **Authoring:** the lab. **Dead-link note:** the source's own labs were on Katacoda (retired 2022) — replaced with Colab.

---

### Session 9 — How LLMs Work: From Neural Networks to Claude
**Goal 3 ("LLM").** The bridge from "a neural network" to "the thing you actually use." ⚠️ **Largely authored** — the corpus's only deep transformer treatment was Cisco-excluded, and the DL course gives transformers one bullet.

- Tokens → embeddings → attention → next-token prediction, at intuition level.
- **Self-attention via one minimal pair:** *"Who is Snow White?"* vs. *"Why is snow white?"* — same words, different meaning, and attention is what tells them apart. (The idea is reusable; build the slide yourself.)
- Why an LLM generates one token at a time; what temperature does; why the context window is finite and costs quadratically.
- **Ties back to Session 1:** now the room can see *why* it hallucinates — it's completing a pattern, and nothing in the mechanism checks the pattern against truth.

**Source:** ❌ transformer internals Cisco-excluded. **Rebuild from (license-checked):** 3Blue1Brown's neural-network/attention series and the *Illustrated Transformer* are excellent but **all-rights-reserved — assign as pre-reading, don't re-slide.** The interactive **Transformer Explainer** (Georgia Tech, Polo Chau) is a strong *live demo*. Verify licenses per asset — research pending session reset.

---

### Session 10 — Prompting I: The Craft *(deepened)*
**Goal 5.** First of two. Move the room from "prompting is typing" to "prompting is a repeatable technique."

- **The prompt-engineering cycle:** define → draft → test → refine → iterate. Prompting as a loop, not a lucky guess.
- The techniques your 2024-era source deck predates entirely: zero-shot vs. **few-shot**, **chain-of-thought**, **system messages**, delimiters, and asking the model to check its own work.
- **Structured output** — getting JSON back reliably — which is what makes prompting useful inside real tooling (relevant to anyone automating release notes, incident triage, config diffs).
- The lever that reframes prompting as engineering: a cheaper model, well-prompted, can match an expensive one — measurably, at a fraction of the cost. *"These nuances are found through testing, not guessing."*

**Source:** Prompt Engineering deck (the cycle + 11-task taxonomy — the durable parts). **Rebuild from (reuse-safe):** OpenAI Cookbook and prompting guide (**MIT**), DAIR.AI promptingguide.ai (**MIT**), The Prompt Report v6 (**CC BY 4.0** — the 58-technique glossary authority). ⚠️ **Do not** base slides on Google's prompting whitepapers (copyrighted, no reuse licence) or Anthropic's interactive tutorial (Claude-3-era, teaches now-obsolete CoT-as-string). **Authoring:** heavy — the source deck contains *zero* actual example prompts.

---

### Session 11 — Prompting II + Working With Claude *(deepened)*
**Goals 5 & 6.** The applied session — general prompting craft made concrete in the tool the team uses.

- **Prompting II:** longer worked examples on the team's own tasks; iterating on a bad prompt live; a small prompt "test set" so a prompt becomes a versioned, checkable artifact rather than folklore.
- **Working with Claude (goal 6 — fully authored, no corpus source):** the scratchpad and how to use it; Projects and persistent context; Artifacts; extended thinking; when to use MCP/connectors; and the workflow habits that separate people who get value from Claude from those who don't. Practical, demo-driven, Qualcomm-task-flavoured (release notes, incident summaries, config review, log triage).

**Source:** ❌ none for the Claude half. **Authoring:** full — I can draft this well; it should be sanity-checked against current Claude documentation at delivery (features move fast), and I can pull the latest specifics after the session-limit reset. **Note:** MCP's final spec lands 2026-07-28; if you cover connectors, land this session after that date.

---

### Session 13 — Risk I: When AI Is Confidently Wrong *(deepened)*
**Goal 7.** First of two. The failure modes that come from the technology itself.

- **Hallucination revisited**, now with mitigations: grounding/RAG, "make the human verify," and the hard truth that *the better the model gets, the harder its rare errors are to catch* — **"if it's right 99% of the time, spotting the 1% is harder, not easier."** For a problem-management audience, that inverts the usual "more accuracy = safer" intuition.
- **Why your metric lies** — the single best teaching asset in the corpus: a model that predicts only people named "Michael" will quit, is wrong on every count that matters, and reports **98% accuracy**. Then base rates and Bayes via the **medical-AI-vendor role-play**: a "99% accurate" test that, once you know the real base rate, is right about **14%** of the time. No code; unforgettable; directly about *evaluating a vendor's AI claim* — which this team will do. ⚠️ **Verified correction:** the source deck prints **3.39%**, but its own expression evaluates to 3.99% *and* its denominator re-imports the vendor's 20%-prevalence sample into a 1% population — worked honestly it is **13.8%**. The deck commits a milder form of the exact fallacy it is exposing. Session 13 keeps 3.39% as the staged reveal and then corrects it live; that correction is the strongest two minutes in the session. Verdict unchanged: six of seven positives are false alarms.
- Human-in-the-loop is necessary and **not sufficient** — you have to check whether the human is actually equipped to catch the error.

**Source:** DL Day 3 §Validation Pitfalls (the medical vendor, the "Michael" parable — complete and excellent as-is). **Authoring:** minimal — swap the medical vendor for an AI-*tooling* vendor pitching the team; the structure transfers exactly.

---

### Session 14 — Risk II: Security, Privacy, and Mitigation in Practice *(deepened)*
**Goal 7, second half.** The failure modes that come from *how you deploy it.* ⚠️ **Largely authored** — your "LLM Safety" deck, despite the name, has zero adversarial-security content.

- **Prompt injection** (direct and indirect) — the vulnerability that has no clean fix, and why it gets *worse* the moment an LLM is wired to act (agents, tools, automated pipelines).
- Data leakage and privacy: what not to paste, what the model retains, PII exposure — squarely relevant to config/release data.
- **A hazard framework the room can reuse** (this *is* in the source, and it's good): the HS/IM/TTO hazard triangle and the operating-domain idea — *make a system safer by constraining it to do less; never let an automated pipeline act on model output without a qualified human gate.*
- **Interactive:** a prompt-injection challenge (e.g. the Gandalf game) — engineers try to break a guarded model live. Nothing teaches "this is not solved" faster than doing it.
- The one hard number the corpus gives: **~39% of AI-generated code suggestions carried a security vulnerability** — *"compiles and works ≠ secure."*
- **EU AI Act** in one honest slide — what actually applies to a company *using* (not building) AI internally, as of 2026.

**Source:** LLM Safety deck (the hazard framework, operating domain); DL Day 3 (Copilot finding). **Rebuild from:** OWASP Top 10 for LLM Applications (current version), Simon Willison's prompt-injection writing, Lakera Gandalf (live). **Authoring:** heavy — all adversarial content. *Security research stream is incomplete (session limit); I'll finish it after reset to pin exact current versions.*

---

### Session 15 — What AI Can and Can't Do — and Will It Take Our Jobs?
**Goals 8 & 9.** The session this audience is actually waiting for. Honest, specific, not reassuring-noise.

- **Capability and its ceiling:** what LLMs genuinely do well (language transformation, drafting, summarising, pattern-spotting over text) vs. where they structurally fail (novel reasoning, guaranteed correctness, anything needing real ground truth). The **S-curve**: capability isn't exponential — *the cost to close the last gap is.* The **proof-of-concept-to-production gap** — the corpus's recurring theme, and precisely this team's professional turf.
- **Will AI take *our* jobs? (fully authored, role by role — no corpus source):** an honest task-level breakdown for **release manager, problem manager, configuration manager, and developer.** The useful framing isn't "replaced / safe" — it's *which sub-tasks get automated, which get augmented, and which get harder because everyone else's output is now AI-shaped.* Release notes drafted, not decided. Incident summaries accelerated, root-cause judgement not. Config drift detected, remediation still owned. Code generated, review and accountability more important, not less. The through-line: **AI changes the composition of these jobs before it eliminates any of them, and the human moves up the stack toward judgement, verification, and accountability** — which is where these roles already live.

**Source:** DL Day 3 (S-curve, production gap, capability limits); LLM Safety (job-loss listed as a hazard). **Authoring:** the entire Qualcomm-role analysis — this is the emotional centre of the series and worth authoring carefully. *I'd want your input on how candid to be.*

---

### Session 16 — What Is AGI, and an Outlook on Quantum
**Goals 10 & 11.** The closer — the big picture and the horizon. ~30 min AGI, ~15 min quantum.

- **AGI (goal 10):** how the definition keeps moving (Turing → "economically valuable work" → skill-acquisition efficiency); the **7 pillars of human intelligence** as a framework for what's still missing (reasoning, memory, learning, language, perception, self-awareness, values); and the honest evidence — including that today's "reasoning" models still fail puzzles children solve, and that **the labs themselves disagree** on whether AGI is even coming (*"AGI is not a moment, it's a transition"* vs. *"I don't believe in God, so I don't believe in AGI"*). The corpus's best material for cutting through hype.
- **Quantum outlook (goal 11 — short, authored, honestly caveated):** what quantum computing is at a high level; where it might intersect AI (optimisation, quantum ML — mostly early research); a realistic timeline; and a straight answer for a Qualcomm audience — **near-term impact on your work is minimal, the intersection with AI is speculative today, and anyone selling "quantum AI" now is selling futures.** Flagged clearly as the most speculative content in the series. *Given Qualcomm's hardware context, tune the emphasis to what's actually relevant to the team — I'd take your steer.*

**Source:** AGI deck (all of the AGI half — your largest, most current source). **Authoring:** the quantum segment in full.

---

## 5. Summary Table

| # | Session | Goal(s) | Primary source | Authoring load |
|---|---|---|---|---|
| 1 | What AI is & how it relates to human thinking | 1 | AGI, LLMSec | Medium (hallucination↔prejudice) |
| 2 | The vocabulary + the cost meter | 2 | LLMSec | **Heavy** (all cost content) |
| 3 | Methods I: learning from data | 3 | DL1 | Light |
| 4 | Methods II: unsupervised learning | 3 | ❌ Cisco → public | **Full** (reuse-safe: scikit-learn) |
| 5 | Methods III: trees & random forests | 3 | ❌ Cisco → public | **Full** (reuse-safe: scikit-learn) |
| 6 | Methods IV: deep learning, conceptually | 3 | DL1–2 | Medium (re-pitch) |
| 7 | Hands-on I: build & train in Keras *(deep)* | 4 | DL1 | Lab authored |
| 8 | Hands-on II: make it better *(deep)* | 4 | DL3 | Lab authored |
| 9 | How LLMs work: from NNs to Claude | 3 | ❌ Cisco → public | **Heavy** |
| 10 | Prompting I: the craft *(deep)* | 5 | PromptEng | **Heavy** (MIT/CC sources) |
| 11 | Prompting II + working with Claude *(deep)* | 5, 6 | ❌ none for Claude | **Full** (Claude half) |
| 12 | Agents and tool use | 5, 6 (ext.) | ❌ none in corpus | **Full** (added post-build) |
| 13 | Risk I: confidently wrong *(deep)* | 7 | DL3 | Light — best asset in corpus |
| 14 | Risk II: security & mitigation *(deep)* | 7 | LLMSec | **Heavy** (all adversarial) |
| 15 | What AI can/can't do + your jobs | 8, 9 | DL3 | **Full** (role analysis) |
| 16 | AGI + quantum outlook | 10, 11 | AGI | Full (quantum) |

**Effort reality check.** Ready or near-ready: **1, 3, 6, 13** (strong material exists). Rebuild from clean public sources: **4, 5, 9, 10** (standard topics, reuse-safe sources identified). Author from scratch — no corpus source: **the cost half of 2, the Claude half of 11, all of 12 (agents), all adversarial security in 14, the role analysis in 15, the quantum segment in 16.** Budget the bulk of prep time there.

---

## 6. Recommendations

1. **Pilot with Session 13.** No authoring, no lab, no code, no currency risk — and it's the strongest material you have, on a topic (evaluating AI vendor claims) this exact audience will use within the month. It'll tell you fast whether the format and appetite are right before you invest in the authored sessions.
2. **Run in order.** The survey builds: vocabulary before methods, methods before hands-on, mechanics before risk, everything before the jobs conversation. Sessions 1 and 15 bookend the emotional arc — "how is this like my mind?" to "what does it mean for my job?"
3. **Session 15 is the one to get right.** For this audience the job question is not academic. Author it honestly and specifically per role; a vague "AI won't replace you, it'll augment you" will not survive contact with a room of experienced managers. I'd want a steer from you on candour.
4. **Fix the source errors before teaching** (listed in `AI_input.md` §6) — the light/dark threshold contradiction and the 4.41/4.1 slope typo will both be caught by a technical Qualcomm audience, and being caught erodes trust in the sessions that follow.
5. **Lab environment is an open decision.** Sessions 7 and 8 assume Google Colab. If Qualcomm policy restricts external notebook hosting, that changes the lab design — flag it early. (JupyterLite runs fully in-browser with no accounts and may sidestep policy issues; pending verification after the research reset.)
6. **Enhancement research is partially complete.** The session hit its API limit (resets 18:40 Berlin) and the RAG, security, transformers, classical-ML, and labs/governance research streams did not all finish. The prompting/agents and RAG-course streams did. **None of this blocks the proposal** — it sharpens the "rebuild from" source list per session. I'll finish it after the reset and fold exact current versions, licenses, and live links into a companion `source_pack.md`.

---

## 7. Open Decisions for You

1. **Candour on Session 15 (jobs).** How direct do you want the per-role analysis? This shapes the tone of the whole series.
2. **Quantum emphasis (Session 16).** Given Qualcomm's hardware remit, is there a specific angle (quantum hardware, post-quantum crypto, quantum ML) you want, or keep it a general horizon-scan?
3. **Lab hosting.** Colab acceptable inside Qualcomm, or do we design labs for a restricted environment?
4. **15 vs. fewer.** This covers all 11 goals with three deepened. If you'd rather compress, Sessions 4–5 (unsupervised + trees) could merge into one "classical ML" survey, giving 14 — at the cost of depth on two of the four required methods.
