# AI_input.md — Structured Extract of `raw_input/`

Consolidated from all 7 PDFs in `C:\Users\hagen\Dropbox\Bibel\AI\raw_input`.
Generated 2026-07-17. Source of truth for downstream training-session design.

---

## 0. Read This First — Four Facts That Shape Everything Downstream

1. **No deck states timings, learning objectives, audience level, or prerequisites.** Only the LLM-safety deck has an objectives slide, and it is five bullets. Every session plan built from this material must author its own agenda, objectives, and level statements. Slide-count proportions (given per deck below) are the only budgeting proxy available.
2. **Exercise prompts are almost entirely missing.** Exercises 1–5 across the Deep Learning series are *title-only slides*. The tasks were delivered verbally or in companion notebooks not present here. All exercise content must be authored from scratch.
3. **One deck is `Cisco Confidential`** (see §1). Licensing decision required before its content is used in delivered training.
4. **The material is not one course.** It is four unrelated products by three authors, spanning beginner ML mechanics to AGI philosophy, recorded 2024–2026. Depth, vintage, and audience differ sharply per deck.

---

## 1. Source Inventory

| # | File | Title | Author | Pages | Vintage | Publisher |
|---|---|---|---|---|---|---|
| 1 | `oreillydeeplearningforbeginnersday1…` | Deep Learning for Beginners — Day 1 | Thomas Nield¹ | 78 | 2024-08 | O'Reilly |
| 2 | `oreillydeeplearningforbeginnersday2…` | Deep Learning for Beginners — Day 2 | Thomas Nield¹ | 84 | 2024-08 | O'Reilly |
| 3 | `oreillydeeplearningforbeginnersday3…` | Deep Learning for Beginners — Day 3 | **Thomas Nield** (named p.85) | 104 | 2024-09 | O'Reilly |
| 4 | `oreillyllmsafetyandsecurity…` | LLM System Safety and Security | **Thomas Nield** (internal evidence) | 122 | ~late 2023 | O'Reilly |
| 5 | `promptengineering…` | ChatGPT Prompt Engineering Cookbook | **Shaun Wassell** | 33 | ~Jan 2024 | webinar (O'Reilly-style) |
| 6 | `masteringaiandmlfundamentals1…` | Mastering the Fundamentals of AI and ML | **Rob Barton & Jerome Henry** | 178 | 2025-05 | **Cisco — see flag** |
| 7 | `agidemystifiedlivesession…` | AGI Demystified — Live Session | **Sinan Ozdemir** | 212 | ~2026 | O'Reilly |

**Total: 811 pages.** ¹ Days 1–2 name no presenter; attribution inferred from the shared running example and Day 3's explicit naming.

> ### 🛑 EXCLUSION — Source #6 (`Cisco Confidential`)
> Every slide of this deck carries a **`Cisco Confidential`** classification banner (MSIP sensitivity label, set 2024-06-11, Cisco tenant). It is companion material to Barton & Henry's published ML book, and both authors are Cisco employees.
>
> **The stated delivery context for this training is an internal Qualcomm team.** Using another company's confidentiality-marked internal material to train a different company's employees is not defensible on the strength of the file being locally available. **Source #6 is therefore EXCLUDED from the training proposal** unless documented permission from Cisco is obtained.
>
> Its content remains extracted below **for private reference only** — it is a useful map of what a complete foundations curriculum contains, and of what must therefore be sourced elsewhere.
>
> **Cost of the exclusion.** Source #6 is the corpus's *only* coverage of: unsupervised learning (K-means, DBSCAN, PCA, t-SNE); decision trees and random forests; classical reinforcement learning (MDP, Bellman, Q-learning, Deep Q); and the best transformer-internals treatment (Q/K/V, multi-head attention, the GPT-3.5 parameter budget). All four are well served by clean public sources — 3Blue1Brown, scikit-learn documentation, Géron's *Hands-On ML*, Bishop, Sutton & Barto, Jay Alammar's *Illustrated Transformer*, and Nield's own published *Essential Math for Data Science*. Rebuilding from those is authoring work, not a content hole.

> ### ⚠️ EXTRACTION METHOD CAVEAT — applies to all 7
> `poppler` / `pdftoppm` is not installed, so PDFs could not be **rendered**; text layers were extracted with `pypdf` / `pdftotext`. **Consequence: purely graphical slides are invisible.** All slide text, code, formulas, tables, and URLs are captured. Diagrams are reconstructed from labels and surrounding prose — descriptions of them are *inferences*. Known total losses: mlfund p.154 (image-only, embedding viz), dl3 p.103 (book covers) and pp.96–98 (NTSB timeline graphics), llmsec resource book covers. Installing `poppler-utils` would enable a visual re-pass.

---

## 2. The Four Products

### 2.1 Deep Learning for Beginners (Days 1–3, 266 pages) — Thomas Nield
*"From Basics to Production with NumPy and TensorFlow."* The only true multi-session course in the set, and the only one with a designed arc.

**The spine:** one example carries all three days — **predict light-vs-dark font for a background colour**, RGB (0–255) → 3→3→1 network. Dataset: `https://tinyurl.com/y2qmhfsr` (1,345 colours, cols 0–2 = RGB, last = binary label).

| Day | Sections | Arc | Ends on |
|---|---|---|---|
| **1** | I Overview & Terminology · II Anatomy of a NN · III Forward Propagation | Hype-deflation → taxonomy → network anatomy → TensorFlow build → from-scratch NumPy forward prop | **A deliberate cliffhanger**: an untrained, randomly-initialised network at ~chance accuracy |
| **2** | IV Derivatives & Gradient Descent · V Stochastic GD · VI Backpropagation | Calculus from scratch → linear regression by GD → chain rule → full from-scratch NumPy backprop | The network finally learns |
| **3** | Applications · Data Prep · Testing & Validation · Validation Pitfalls · Production · Larger Systems · Uber case study | *Doing well on a test set is not the job* | A person who died because a system had no guardrails |

**Day 3 is only ~25% code.** Pages 2–42 are technical; pages 43–101 are a sustained critical-thinking arc with essentially no code. **It splits cleanly into two sessions at p.43.**

**Pedagogical devices worth stealing (the most reusable asset in the whole corpus):**
- *One running example across a whole build* — f(x)=x² for derivatives; (x−3)²+4 for 1-var GD; y=mx+b throughout Section V.
- *"Let's walk this through"* — one code block held constant across 4–5 slides, a different line explained each time. Designed for type-along delivery.
- *Estimate first, then formalise* — finite-difference slope by hand **before** SymPy's `diff`; sum-of-squares in a table **before** the GD code.
- *Rejected-alternatives framing* — argues why **not** to sum residuals, why **not** absolute values, before landing on squares.
- *Metaphor as load-bearing intuition* — flashlight-in-the-mountains for GD; "giant vs. ant" for learning rate; "nested onion" for layer derivatives; slope as compass.
- *Socratic prompts left hanging* — "How do you think we can estimate this slope?", "What defines a best fit anyway?", "How do we escape?"
- *Cognitive-load release beats* — "Congrats! You are doing multivariable calculus."; a full slide reading **"WHEW!"** after the dense derivation.
- *Library-first, then from-scratch* — TensorFlow (p.38) **before** NumPy (p.77). Stated explicitly as a choice.
- *Front-loaded skepticism* — ~⅓ of Section I deflates AI hype. The instructor builds credibility by arguing **against** the technique he is teaching.

**Key technical content:** activation functions (ReLU/sigmoid/tanh/softmax + selection table); weights/biases; Z1/A1/Z2/A2 formulation; `@` matmul and the rows=nodes convention; derivatives via SymPy; learning rate & epochs; loss/residuals/sum-of-squares; convex vs. non-convex; SGD vs. mini-batch; the chain rule and the "diagonal cancellation" mnemonic; full `backward_prop()` implementation.

**Day 3's strongest assets, ranked:**
1. **The medical-AI-vendor scenario (pp.50–55)** — a base-rate-fallacy lesson disguised as a procurement decision. Vendor claims 99% sensitivity; outside data reveals a 1% population base rate vs. 20% in their test sample; the deck reports P(at risk | positive) = **3.39%**. Ends in a decision. The single best teaching asset in the corpus. ⚠️ **But the deck's own numbers are wrong (verified by computation):** `.99 × .01 / .248` = **3.99%**, not 3.39%; and `.248` is the positive rate in the vendor's *enriched 20% sample*, not in a 1% population — the honest answer is **13.8%**. The deck thus commits a gentler version of the fallacy it is teaching. Correct it when teaching; the correction is a feature, not a defect. Verdict unchanged: six of seven positives are false alarms.
2. **The Uber Tempe teardown (pp.88–101)** — forensic walkthrough of the 2018 fatality with NTSB quotes. Classification flickers between "unknown/vehicle/bicycle"; emergency braking **disabled** to reduce false alarms; the system "did not include consideration for jaywalking pedestrians." Ends in a colour-coded hazard diagram and 4 lessons.
3. **The "Michael" accuracy parable (p.36)** — a model predicting only people named Michael quit, wrong on both counts, boasting **98% accuracy**. Full confusion matrix worked on p.38: sensitivity 0, precision 0, F1 **undefined**, accuracy .98.
4. **The manifold hypothesis build (pp.19–25)** — 6-slide geometric walkthrough; why deep learning works.
5. **The S-curve (pp.64–65)** — "it is not AI capability that is exponential, but rather the expense to make it."
6. **The p-hacking motivations slide (p.58)** — *"No paper, no funding"* / *"Our client wants to see 10% savings"* / *"Our VC investors want a demonstration."* Framed as **"not usually malicious — human nature under pressure."**

Other Day 3 material: Bayes via the violence/video-games case (P(homicidal|gamer) = **0.02%**); p-values via Fisher's lady-tasting-tea; selection bias (the Volvo kangaroo); adversarial attacks (panda→gibbon, stop-sign stickers); data rot; the data-labelling economy; modular vs. end-to-end; the DARPA Heron dogfight exercise with a 6-point debrief; Chollet and Andrew Ng pull quotes.

---

### 2.2 LLM System Safety and Security (122 pages) — Thomas Nield

> **This is NOT an adversarial-security course.** It contains **zero** prompt injection, jailbreaking, OWASP LLM Top 10, red-teaming, guardrail tooling, RAG-specific attacks, or PII-leakage taxonomy. "Security" is used in the **system-safety / stakeholder-protection** sense. Data poisoning gets one joking sentence. **If adversarial security is needed, it must be written from scratch.**

**Stated objectives (the only such slide in the corpus):** (1) brief explanation of LLMs; (2) ground truth and trust; (3) operating domain and hazards/risks; (4) use cases and broader issues; (5) organisational policy.

**The transferable framework — aviation/automotive system safety ported onto LLMs. This port is the deck's original contribution:**

- **The Hazard Triangle (HS/IM/TTO)** — every hazard has exactly three components: **Hazard Source** (the energy source), **Initiating Mechanism** (the trigger), **Target/Threat Outcome** (who's vulnerable, the threat, the severity). Two rules: *reduce any one component → shrink the triangle → reduce risk; eliminate any one → the triangle collapses → the risk disappears.*
- **The key abstraction:** for an LLM, the "energy source" reduces to **a piece of wrong/suboptimal information.**
  - *Hazard Sources:* training data, architecture, user prompt, unverified output.
  - *Initiating Mechanisms:* an untrained overtrusting human; **an API that directly acts on LLM output**; poor definition of sanctioned use; poor data privacy; **a financial environment incentivising hype**.
  - *Target/Threat Outcomes:* three victim tiers — the individual, the business, society.
- **Operating Domain** — the core prescription. Map (1) what data goes in, (2) what tasks it is sanctioned for, (3) how the user is trained to verify. Chain: `Data → LLM → User → Real-world Actions`. Exemplar: a law-firm LLM restricted to internal case documents, used by expert lawyers, **explicitly barred from discovery and from searching the internet**.
- **The Swiss cheese model** — *"You make a system safer by constraining it to do less."*
- **"Think in systems, not tasks!"** — the app boom created a task-oriented mindset where the task exists in a bubble. Ask not "how do I get better at this task?" but "how does this task impact the system around it?"
- **The human-factor trap:** *"If an LLM is performing well 99% of the time, it becomes that much harder for the human to identify that 1%."* System safety has shown humans are poor at catching infrequent automation errors (startle factor).

**The decision rule (compact enough to be a session's central takeaway):** an LLM application is defensible when **(a) the user can easily verify the output**, or **(b) truth is irrelevant** (fiction, art) — and is made safer by shrinking the operating domain and never letting an API act on output without a qualified human. **The verification paradox:** *the more the LLM improves, the more work there is in verifying the output.*

**How LLMs work — the 4-step teaching progression:** frequency/probability model (all Beatles lyrics → combinatorial explosion) → word embeddings (Word2Vec; 2-D scatter of Beatles words) → attention / filling blanks (`___ want __ hold your __?__`) → **"an LLM is an autocomplete on steroids... a pattern-spotting and matching engine, not a search engine looking up facts."**

**Ground truth:** the pedestrian paradox — *"When a self-driving car has failed to recognise a pedestrian, how can it recognise that it failed?"* Five questions to interrogate any corpus. **Interpolation vs. extrapolation** (LLMs good at the former, bad at the latter) — which grounds the hallucination mechanism: *hallucination happens because the model extrapolates into sparse, brittle space.*

**Trust:** human-in-the-loop is necessary but **not sufficient** — you must evaluate whether the person is *fit* to evaluate. Benchmarks treated dubiously ("like giving the answers to the test to an AI"). **"Passing the Bar ≠ Practicing Law"** — memorise → interpolate → extrapolate spectrum. Garbage-in-garbage-out via Grok trained on X posts. Data drift.

**Hallucination taxonomy:** intrinsic (inconsistent with in-context source → RAG's failure mode) vs. extrinsic (unverifiable, from pre-training).

**Interactive format — highly reusable:** **7 use cases**, each a scenario slide → *"Is the spirit of this application A) Safe B) Unsafe C) It depends"* → poll the room → **"My opinion:"** reveal. Verdicts deliberately ordered so the room cannot pattern-match: *depends → safe → safe → safe → UNSAFE → safe → UNSAFE.*

| # | Case | Verdict |
|---|---|---|
| 1 | Copilot-style code generation | **It depends** — IEEE S&P 2022 study: **39.33%** of top suggestions led to vulnerabilities. *"Just because your code compiles and 'works' does not mean it is secure."* |
| 2 | Anti-scam time-wasting bot | **Safe** — *"might be the golden application for an LLM"* |
| 3 | Email tone rewriting | **Safe** — the draft-first requirement **is** the safety control |
| 4 | Creative-writing assistant | **Safe (mostly)** — but the Clarkesworld submission flood |
| 5 | AI romantic companion (Replika) | **UNSAFE — "Just, no."** Chail/Queen assassination case |
| 6 | Recipe generator | **Safe** — teaches *proportionality*: poisoning is real but the TTO is trivial |
| 7 | "LLMTraderBotPro" stock picker | **UNSAFE** + probably a scam |

**3 exercises with full debriefs** (the only fully-specified exercises in the corpus): customer-service automation; EULA verification; AI paralegal.

**Best demo in the corpus:** ChatGPT's **hallucinated biography of the presenter himself** — fabricated degree, employers, a consultancy, and two books he never wrote. Makes undetectable hallucination visceral.

**Broader issues:** data-labelling labour ethics; the Spampocalypse (AI-generated **mushroom-foraging books** on Amazon — misinformation → physical harm) and the **ouroboros** (LLM output → scraped → training input); "using AI to detect AI" — *short answer, no*; the student problem; **the Economics of Hype** 6-step bubble paradigm; regulatory capture; Munger's *"Show me the incentive, I will show you the outcome."*

**Content gaps:** the policy section is exhortation and mindset, **not a template or checklist** — despite policy being stated objective #5. No evaluation methodology, no incident response, no technical controls, **no agents/tool-use** (the "API that directly acts on an LLM" bullet is prescient but undeveloped).

**Structural note:** footer slide numbers diverge from PDF pages (gaps at 3-4, 6-9, 23-26, 32-36, 61-64, …). This export is a **condensed subset of a ~120+-slide master**; roughly 25–30 slides are missing.

---

### 2.3 ChatGPT Prompt Engineering Cookbook (33 pages) — Shaun Wassell

**The thinnest source by a wide margin, and a scaffold rather than a resource.** ~40% of slides are section dividers, breaks, and Q&A. The teaching happened live; nothing was captured.

**Two reusable assets:**

1. **The Prompt-Engineering Cycle** — a 6-step loop: define the objective → develop initial prompts → test and evaluate → refine → iterate → final evaluation.
2. **The 11-task-type taxonomy** — the "Cookbook" itself. Each type gets prompt-design principles + 3 example use-cases: Transformational · Creative · Critical Thinking · Procedural · Content Generation · Data Analysis · Role-Playing · Code Generation · Language Translation · Educational · Recommendation. Model-agnostic, cleanly templated, and maps naturally onto exercise design (one drill per type).

**Notable individual principles:** avoid yes/no questions for critical thinking; **ask it to analyse its own responses** (self-critique); specify current skill level for educational tasks; sequence matters for procedural tasks.

**Severe problems for reuse:**
- **Zero verbatim prompts.** For a deck titled "Cookbook" there are no recipes — only recipe *categories* ("Summarizing a Long Article into Bullet Points").
- **Section 5 is 75% unbuilt.** Four sub-topics advertised (Custom GPTs, OpenAI API, Playground, **System Message**); one content slide delivered. The advertised "Advanced Prompt Engineering" segment effectively does not exist — and the system message is the most valuable of the four for a modern audience.
- **The vocabulary is pre-CoT-era.** Absent: *zero-shot / few-shot / chain-of-thought / ReAct / temperature / tokens / context window / RAG / fine-tuning / hallucination / prompt injection / delimiters / output schemas.* **This is the single biggest content gap in the corpus.**
- Dated in specifics ("Custom GPTs still in Beta as of January 2024"); durable in principles.

**One concrete asset:** ELIZA (1966), live and clickable at `https://web.njit.edu/~ronkowit/eliza.html` — a natural "try this, then compare with ChatGPT" opener.

---

### 2.4 Mastering the Fundamentals of AI and ML (178 pages) — Barton & Henry ⚠️ `Cisco Confidential`

The broadest survey in the corpus, and the only source covering classical ML beyond neural networks.

**8 sections:** Introduction to AI/ML · Unsupervised Learning · Regression · Classification · Decision Trees · Reinforcement Learning · Neural Networks · Generative AI.
*Slide-count proportions: S1 13% · S2 12% · S3 12% · S4 7% · S5 11% · S6 13% · S7 14% · S8 17%.*

**Unique coverage not available anywhere else in the corpus:**
- **Unsupervised learning** — K-means (6-slide animated build; centroids → assign → recentre → repeat to convergence), the elbow method + SSE, silhouette; **DBSCAN** (MinPts, ε, core/border/noise points, parameter sensitivity); the curse of dimensionality; **PCA** (eigenvectors; the "press the object" intuition) and **t-SNE**. Rule of thumb: *use t-SNE to visualise, PCA to compute.*
- **Decision trees and random forests** — the classic "buys_computer" worked example; **Gini impurity** hand-computed; bootstrap; **bagging**; out-of-bag data and forest efficiency; inference by majority vote.
- **Reinforcement learning** — agent/environment/state/action/reward/policy; **MDP** (5 components) with a job-promotion worked graph; the **Bellman equation**; **Q-learning** with α; **Q-tables**; **Deep Q-Learning**; the discount factor motivated by the **CoastRunners** reward-hacking case (worked number: 100 points → 81 two years out at γ=0.9).
- **Transformer internals at depth** — tokenisation; word embeddings (Word2Vec, 100–300 dims); positional encoding; **Q/K/V** roles worked through "I love summer"; multi-head attention (h=8, n×d/8 splits); encoder vs. decoder (auto-encoding BERT vs. auto-regressive GPT); residual connections; autoregressive generation to EOS; **softmax temperature**; "ChatGPT has 96 transformer layers."
- **The GPT-3.5 175B parameter budget table (p.178)** — a full component-by-component breakdown summing to 175,181,291,520. d_model=12,288 · vocab=50,257 · 96 heads · 96 layers · head dim 128 · FFN inner 49,152. An excellent capstone.
- **CNN mechanics** — the X-vs-O build: three 3×3 kernels → feature maps → ReLU cleanup → pooling → 9×9 down to 2×2.
- **GANs** (one architecture slide) and **RNN/LSTM** (forget-gate intuition only, no cell-state math).
- **Flagship self-attention example:** *"Who is Snow White?"* vs. *"Why is snow white?"* — identical embeddings, different meanings after attention. Genuinely excellent.

**The deck's strongest pedagogical device:** *cost/distance* reused as a spine across the whole course — MSE (regression) → log-loss (classification) → **Gini impurity** (*"the same concept of 'distance' or 'cost' we saw before, applied to probabilities"*) → Bellman/Q-values → backprop error.

**Other assets:** the 1906→2025 papers/events timeline (dense but complete, incl. both AI winters); the inverted-inputs framing (*rules-based:* data + program → output; *ML:* data + output → **program**); parameters vs. hyperparameters; the 70/15/15 train/validation/test split; 5 DEMO slides (hyperparameters, linear regression, sigmoid, Q-learning tic-tac-toe — repo `github.com/Rohithkvsp/Tic-Tac-Toe-Reinforcement-learning`); two self-serve tools (`platform.openai.com/tokenizer`, `projector.tensorflow.org`).

**Errors to fix before reuse:** **SVM is a stated objective and named as *the* multiclass algorithm — but has zero content slides** (largest gap); "GAN/Deepfakes" promised, only one GAN slide, no deepfakes; section-numbering errors (p.125 "Section 5" inside Section 7; p.173 "Section 1" inside Section 8); p.58 mixes examples (study-related features, braking-distance equation); p.79/80 swap the Hypothesis and Cost labels; typos p.28.

---

### 2.5 AGI Demystified (212 pages) — Sinan Ozdemir

The largest, most current, and most conceptually ambitious deck. **Structural conceit: Sinan's 7 Pillars of Human Intelligence** — Reasoning · Memory · Learning · Language · Perception · Self-Awareness · Motivation/Values. Each pillar follows a repeated three-beat template: **Classical View** (philosophy) → **Modern View** (cognitive science) → **Relevance to AGI** (ML practice + evidence).

**Beyond the pillars:** world models (LeCun's V-JEPA 2, DeepMind's Genie 3) · benchmarks & evaluation · advanced reasoning models (prompting, RLHF, PPO, GRPO) · multimodal & computer use · AI agents · limits of the Transformer.

**Most valuable for a developer audience — the modern-practice material absent everywhere else in the corpus:**
- **Prompting done properly:** few-shot / in-context learning; **chain of thought**; **semantic few-shot** (store examples in a vector DB, retrieve dynamically). Key finding: *V3 (non-reasoning) can be made as good as R1 with a bit of prompting — at half the cost.* *"These nuances are often found through testing."*
- **Agents:** the definition (semi-autonomous: autonomy + decision-making + adaptation); **ReAct** and the Thought–Action–Observation loop; **Plan & Execute** (a large LLM plans, small fast LLMs execute); **Reflection** (a critique module before the final answer); Deep Research as the synthesis of all three; LangChain/LangGraph.
- **Computer use — a clean two-way taxonomy:** *Truly Multimodal* (an LMM parses the screen directly — Anthropic/OpenAI computer use, Nova Act) vs. *Grounded Textual* (DOM → element list → LLM — browser-use + Playwright). Plus OmniParser and Qwen2.5-VL bounding boxes.
- **RLHF/RL in depth:** three phases; the reward-model loop; **PPO pseudocode**; the **KL penalty** (constrains fine-tuning so the model doesn't output gibberish to fool the reward model); combined reward terms; practical warnings (easy to overfit imperfect reward models; runs take days; numerical instabilities).
- **Evaluation and probing:** **probing** = testing what knowledge is encoded in a parameter set; calibration (a 60%-confidence prediction should be right ~60% of the time; **fine-tuning and prompting both induce calibration**); token confidence as a hallucination proxy; intrinsic vs. extrinsic hallucination.
- **Benchmark catalog:** TruthfulQA · MMLU · MTEB · Humanity's Last Exam · **ARC-AGI** (skill-acquisition efficiency; o3 ~75% on ARC-AGI-1; **ARC-AGI-2 human-solvable, still AI-unsolved**) · GDPval · SWE-bench · GAIA · MMMU.
- **The four-question benchmark checklist (p.138) — directly reusable as a rubric:** (1) Who made it, and what was their intent? (2) What is it testing, and do I care? (3) How often is it updated? (4) Is it measuring what it claims to measure?

**5 "Code Time!" labs:** Intro to Reasoning Models · Basic Memory with OpenAI's Agents SDK · Benchmarking models · Teaching an AI to reason with GRPO · *Attempting* to build computer use with reasoning models.

**The deck's defining virtue — honest negative results.** It repeatedly leads with what *didn't* work: reasoning didn't help on a simple task; **o3/o4-mini hallucinate more than 4o**; the agentic notepad only helps when future tasks resemble past ones; the chess world-model probe beats random by only ~10%; reasoning costs severe latency; R1 burns ~1,700 thinking tokens where Claude uses ~300.

**Other threads:** definitions of AGI over time (Turing → Newell & Simon → Legg & Hutter → Goertzel → Chollet → OpenAI → ARC-AGI); Altman's 5 Levels (with *"arguably we are here now"* between Agents and Innovators); the Extended Mind Thesis (Clark & Chalmers' Otto — the philosophical warrant for treating RAG/tools as genuine memory); episodic vs. semantic memory; Searle's Chinese Room vs. Dennett's rebuttal; value pluralism and cultural bias in alignment; **benchmark skepticism as a through-line** ("OpenAI makes benchmarks to measure AI success on economically valuable tasks — *how convenient*"; "many popular benchmarks are made by institutions that also make models"); what the labs themselves say (Amodei: *"AGI is not a moment—it's a transition"*; LeCun: *"smart parrots"*; Mensch: *"I don't believe in God… so I don't believe in AGI"*); the five limits of the Transformer (O(n²) scaling · shallow vectorised reasoning · architectural rigidity · no embodiment · intellectual monoculture).

---

## 3. Cross-Cutting Topic Map — Where Each Topic Lives

| Topic | Primary source | Also in | Depth |
|---|---|---|---|
| AI/ML/DL taxonomy, history | mlfund S1 | dl1 §I, agi §1 | Deep (3× redundant) |
| Hype deflation / realism | dl1 §I, dl3, llmsec | agi | **Very deep — a corpus-wide obsession** |
| Unsupervised (K-means, DBSCAN, PCA, t-SNE) | **mlfund S2 only** | — | Deep — ⚠️ Cisco-only |
| Regression, cost functions, gradient descent | dl2 §IV, mlfund S3 | — | Deep (from scratch, twice) |
| Classification, sigmoid, logistic regression | mlfund S4, dl1 | — | Medium |
| **SVM** | — | — | **ABSENT (promised, never delivered)** |
| Decision trees, random forests | **mlfund S5 only** | — | Deep — ⚠️ Cisco-only |
| Reinforcement learning | **mlfund S6**, agi (RLHF) | dl3 | Deep — classical RL ⚠️ Cisco-only |
| NN anatomy, activations, forward prop | dl1 §II–III | mlfund S7 | **Very deep** |
| Backprop, SGD, chain rule | dl2 §V–VI | mlfund p.137-138 | **Very deep (from scratch)** |
| CNN / RNN / LSTM / GAN | mlfund S7, dl3 p.3 | — | Medium — ⚠️ mostly Cisco |
| Data prep, scaling, feature selection | dl3 §2 | mlfund | Medium |
| Validation, k-fold, confusion matrix, ROC/AUC | **dl3 §3** | mlfund p.26 | **Deep** |
| Bayes, base rates, p-hacking | **dl3 §4** | — | **Deep — outstanding** |
| Production failure modes | **dl3 §5** | llmsec | **Deep** |
| Modular vs. end-to-end systems | dl3 §6 | llmsec | Deep |
| Transformer internals (Q/K/V, attention) | **mlfund S8**, agi §Language | dl3 (1 bullet) | Deep — ⚠️ best treatment is Cisco |
| Tokenisation, embeddings, context windows | mlfund S8, agi | llmsec | Deep |
| Prompt engineering | prompteng, **agi** | llmsec | **Shallow-to-medium — see gaps** |
| RLHF / PPO / GRPO / alignment | **agi only** | — | Deep |
| Agents, ReAct, tool use | **agi only** | — | Medium-deep |
| Computer use / multimodal | **agi only** | — | Medium |
| Evaluation, benchmarks, probing, calibration | **agi**, dl3 | llmsec | Deep |
| Hallucination | llmsec, agi | — | Deep |
| LLM system safety (HS/IM/TTO, operating domain) | **llmsec only** | dl3 | **Deep — the framework** |
| **Prompt injection / jailbreaking / OWASP LLM Top 10** | — | — | **ABSENT — must be authored** |
| **Guardrails / evals tooling / red-teaming** | — | — | **ABSENT — must be authored** |
| **RAG (architecture, chunking, retrieval quality)** | — | agi (mentions only) | **NEARLY ABSENT** |
| **AI-assisted coding practice** | — | llmsec Case #1 | **ABSENT** |
| **Cost / latency / model selection engineering** | agi (scattered) | — | Thin |
| Ethics, labour, societal impact | llmsec, dl3, agi | — | Deep |
| Org policy | llmsec §8 | — | **Exhortation only, no template** |

---

## 4. Gap Register — What Must Be Authored From Scratch

**Ranked by how badly a 2026 developer audience would feel the absence.**

1. **Adversarial LLM security in its entirety** — prompt injection (direct and indirect), jailbreaking, OWASP LLM Top 10, red-teaming, guardrails, output filtering, tool-use/agent attack surface, supply chain. The safety deck's framework is sound but its "API that directly acts on an LLM" bullet is one line where a modern course needs a whole session.
2. **Modern prompt engineering vocabulary and practice** — zero/few-shot, chain-of-thought, structured outputs/JSON schemas, delimiters, temperature, context windows, system messages, evaluation of prompts. The prompt-engineering deck predates all of it; the AGI deck covers CoT/few-shot but as AGI evidence, not as craft.
3. **RAG** — architecture, chunking, embedding choice, retrieval quality, intrinsic hallucination in RAG. Mentioned across three decks, taught in none.
4. **All exercise prompts** — Exercises 1–5 are title-only slides.
5. **Agentic patterns as engineering** — the AGI deck covers ReAct/Plan-Execute/Reflection conceptually; nothing covers building, testing, or bounding them in production.
6. **Org policy templates** — stated as an objective in the safety deck, delivered as five principles.
7. **SVM** — promised in the Cisco deck, never delivered.
8. **Timings, objectives, prerequisites, level statements** — for every session.
9. **AI-assisted coding as a discipline** — highly relevant to this audience; only the 39.33%-vulnerability finding exists.

---

## 5. Currency Register — What Has Aged, and How

| Item | Source | Status |
|---|---|---|
| Google **Bard** demos (all of them) | llmsec | **Dead.** Bard→Gemini. Demos must be re-run. |
| **Katacoda** hands-on scenarios (2) | dl3 pp.7, 41 | **Dead** — Katacoda retired 2022. Port to Colab/Jupyter/Binder. The code itself still runs. |
| `RFE(model, 2)` positional arg | mlfund/dl3 p.15 | Deprecated — now needs `n_features_to_select=2` |
| Transformers get **one bullet** | dl3 p.3 | Badly dated for 2026 |
| ChatGPT treated as novel | dl3 p.4, llmsec | Dated framing |
| "Custom GPTs still in Beta (Jan 2024)" | prompteng | Dated |
| The **S-curve** argument | dl3 pp.64–65 | Predates the LLM scaling era — **this makes it a *better* discussion slide, not a worse one.** Invites direct challenge. |
| "L5 self-driving isn't there yet" (2019 quote) | dl3 p.74 | Needs a 2020s update — Waymo is commercially deployed |
| Low-interest-rate premise of the hype paradigm | llmsec | Dated |
| **EU AI Act never mentioned** | llmsec | Significant omission for 2026 |
| "AI to detect AI — short answer, no" | llmsec | Worth re-checking |
| MSE loss with softmax/sigmoid classifiers | dl1, dl3 | Unconventional — categorical cross-entropy expected. Likely a deliberate simplification to keep one loss function across the course; **flag if reused.** |

---

## 6. Error Register — Source Defects to Fix Before Teaching

| # | Source | Defect |
|---|---|---|
| 1 | dl1 p.26 vs p.35 | **Contradiction:** p.26 says output ≥.5 → **DARK**; p.35 says ≥.5 → **light**. p.26 matches the stated "probability of predicting dark font." **Resolve before teaching.** |
| 2 | dl1 p.30/49 vs p.77 | Weights stated as initialised **−1 to 1**, but `np.random.rand` yields **0 to 1**. |
| 3 | dl1 p.40 | `confusion_matrix` imported but never used — a natural place to add a segment, or remove. |
| 4 | dl2 p.6 | **Typo:** rise-over-run slope printed as **4.41**; correct value is **4.1**. |
| 5 | dl1 p.2 | Typos: "Backprogation", "dep learning". |
| 6 | mlfund p.70/73 | **SVM stated as an objective and named as the multiclass algorithm — zero content slides.** |
| 7 | mlfund p.125, p.173 | Section-numbering errors ("Section 5" inside S7; "Section 1" inside S8). |
| 8 | mlfund p.58 | Feature list is study-related; equation is labelled "Breaking Distance". |
| 9 | mlfund p.79/80 | "Hypothesis" and "Cost" labels swapped. |
| 10 | mlfund p.83 | DeepSeek "R3" — likely means R1/V3. |
| 11 | mlfund p.28, p.91 | Typos "Woks"/"advnatages"; Gini transcription slip printing 0.5 for a pure split. |
| 12 | prompteng | Agenda promises "Advanced Prompt Engineering"; Section 5 is titled "Environment Improvements" and is 75% unbuilt. |
| 13 | dl1 p.16 vs the course's own example | DL defined as ">1 hidden layer", but the running example has **one** hidden layer — so the course's own network **is not deep learning**. Worth calling out explicitly rather than hiding. |
| 14 | dl3 pp.27–28 | Duplicate slides (likely an animation build). |

---

## 7. Asset Index

### Datasets
| URL | Content | Used by |
|---|---|---|
| `https://tinyurl.com/y2qmhfsr` | **1,345 colours, RGB + binary light/dark label** — the spine of the whole DL course | dl1 pp.38/40/77, dl3 p.41 |
| `https://tinyurl.com/yaxgfjzt` | Linear regression, headerless | dl2 pp.34–38 |
| `https://bit.ly/3n1FKgE` | SGD dataset, with header | dl2 pp.56–57 |
| `https://tinyurl.com/y6r7qjrp` | MNIST-style CSV with `class` column | dl3 pp.32, 35 |
| `https://bit.ly/39RbG1D` | Iris | dl3 pp.10, 15 |
| `https://bit.ly/33iTfS9`, `https://bit.ly/3flZJSR`, `https://bit.ly/3i4cUcE` | Tabular (scaling, normalisation, PCA demos) | dl3 |
| `hf.co/datasets/MacPaw/UiPad` | 228 screens, computer-use tasks | agi p.21 |
| BIRD | 12,000+ text-to-SQL pairs, 95 DBs, 33 GB | agi pp.31–33 |
| MathQA, PersonQA, App Reviews | reasoning / hallucination / calibration | agi |

### Interactive tools
| Tool | Purpose |
|---|---|
| `desmos.com/calculator/jwjn5rwfy6` | Activation functions |
| `desmos.com/calculator/ruvtlp9zk6` | Gradient descent slope/compass |
| `desmos.com/calculator/fmhotfn3qm` | Linear regression best fit |
| `desmos.com/calculator/wmwfolbvdk` | Overfitting / bias-variance |
| `geogebra.org/calculator/xujf9z9y` | Manifold projection in 3-D |
| `platform.openai.com/tokenizer` | Tokenisation (self-serve) |
| `projector.tensorflow.org` | Word2Vec embedding space (self-serve) |
| `web.njit.edu/~ronkowit/eliza.html` | ELIZA (1966), live |
| `ml4a.github.io/ml4a/looking_inside_neural_nets/` | NN weight heat maps |
| `colah.github.io/posts/2014-03-NN-Manifolds-Topology/` | Manifolds and topology |
| ~~`katacoda.com/orm-thomas-nield/...`~~ | **DEAD** (2 scenarios) |

### Key citations
Choromanska et al. 2014, *The Loss Surface of Multilayer Networks* (arXiv 1412.0233) · Yurtsever et al. 2020, *A survey of autonomous driving*, IEEE Access 8 · NTSB HAR1903 (Uber Tempe) · Ioannidis 2005, *Why most published research findings are false* · arXiv 2108.09293 (Copilot code security, IEEE S&P 2022) · arXiv 2311.00388 (interpolation/extrapolation) · Ouyang et al. 2022 (InstructGPT, arXiv 2203.02155) · Hendrycks et al. 2020 (MMLU) · Lin et al. 2022 (TruthfulQA) · Clark et al. 2019 (*What Does BERT Look At?*) · LeCun 2022 (*A Path Towards Autonomous Machine Intelligence*) · Apple, *The Illusion of Thinking* · `arxiv.org/abs/2505.14178` (tokenisation vs. symbolic reasoning) · `github.com/fchollet/ARC-AGI`

### Recurring voices (usable as pull quotes)
**Andrew Ng** (the Stanford-radiology data-drift quote — appears in *two* decks; the proof-of-concept-to-production gap) · **François Chollet** (end-to-end DL can't generalise; benchmark measure-gaming; *"what would I have learned about intelligence? Well, nothing"*) · **Yann LeCun** (world models; "smart parrots") · **Narayanan & Kapoor / AI Snake Oil** ("memorization is a spectrum") · **Gary Marcus** · **Charlie Munger** ("show me the incentive…") · **Ronald Coase** ("if you torture the data long enough, it will confess") · **Dario Amodei** ("AGI is not a moment—it's a transition") · **Arthur Mensch** ("the pursuit of AGI feels very religious to me")

---

## 8. Corpus-Level Observations for Session Design

1. **Three of the four products share a single thesis:** *AI capability is systematically over-claimed, and the interesting engineering is in the gap between benchmark and reality.* The DL course front-loads it, Day 3 is built on it, the safety deck formalises it, and the AGI deck leads with negative results. **This is a coherent editorial voice and the corpus's greatest strength — a series built from it would have a spine, not just a syllabus.**
2. **The corpus is strong where courses are usually weak** (epistemology, validation skepticism, production failure, system safety, ethics) **and weak where courses are usually strong** (hands-on labs, exercise prompts, modern tooling). Plan accordingly: the *thinking* is largely done; the *doing* must be built.
3. **Coverage is deep on 2024-era foundations and thin on 2026-era practice.** Backprop from scratch: excellent. Prompt injection, RAG, agent engineering: absent. For a developer audience, the second list is what they'll ask about.
4. **Thomas Nield wrote 4 of the 7 decks (388 pages, 48%).** His voice, running examples, and pedagogical devices dominate. The Andrew Ng radiology quote and the confusion-matrix material appear in two decks — deduplicate if sequencing them together.
5. **Redundancy to exploit, not fight:** the AI/ML/DL taxonomy appears three times; gradient descent twice (dl2 from scratch, mlfund conceptually); transformers twice (mlfund mechanically, agi conceptually). Pick the better treatment per topic rather than merging.
6. **The best single hour in the corpus** is arguably dl3 pp.50–55 (medical AI vendor) → pp.88–101 (Uber Tempe): base-rate fallacy → real fatality → hazard diagram. It requires no code and lands with any technical audience.
7. **Two formats are proven and directly liftable:** the A/B/C "Safe/Unsafe/It depends" case poll (7 instances, verdicts deliberately unpatterned) and the "let's walk this through" repeated-code-block build.

---

## 9. Provenance

Per-deck full extractions (page-by-page outlines, verbatim code, complete link indexes) live in the session scratchpad:
`…\40711341-d039-4ae4-af3c-78a66921f118\scratchpad\extract_{agi,dl1,dl2,dl3,llmsec,mlfund,prompteng}.md`
This file consolidates them; the extracts carry detail deliberately omitted here.
