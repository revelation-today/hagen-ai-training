# Discussion & Polls — Session 7

For the 15-minute Q&A block and the in-session polls. Each prompt notes what a good answer surfaces. Prompts run from concrete (about the lab just completed) to applied (about the room's actual work).

## Live polls (during the lab)

Short, fast, hands-up or a Colab-cell vote. Their purpose is to make the room **commit to a prediction before the reveal** — a fact you predicted wrongly sticks; a fact you were told slides off.

**Poll 1 — before the build cell.**
> How many numbers do you think this network contains? **(A)** about 16 · **(B)** about 1,000 · **(C)** about 100,000

- *Answer: A — exactly 16.* Most rooms guess far too high, which is the useful part. Derive it live (9 + 3 + 3 + 1) and let the smallness land. Then: a frontier LLM is this exact structure with a few hundred billion, and the mechanism is unchanged.

**Poll 2 — immediately before `evaluate()` on the untrained model. (The important one.)**
> The model is built but has never seen data. What accuracy will it get on the test set? **(A)** ~0% · **(B)** ~50% · **(C)** ~96%

- *Answer: B, roughly — but the actual number is ~55%, and the discrepancy is the whole lesson.* Take the vote, then reveal 0.549, then ask "why isn't it exactly 50?" before answering. It predicted one class for **every** colour, and that class is 55% of the test set. The 5 percentage points above chance are arithmetic about the dataset, not skill. Do not rush this.

**Poll 3 — before the 5-epoch re-run.**
> With only 5 epochs instead of 100, the model will score: **(A)** about the same · **(B)** noticeably worse · **(C)** better (less overfitting)

- *Answer: B (~0.72).* C is the interesting wrong answer — some people have heard "too many epochs causes overfitting" and over-apply it. Use it to name **underfitting** and set up Session 8's overfitting opener as the mirror image.

**Poll 4 — before predicting mid-grey (128,128,128).**
> For mid-grey, will the model's probability be **(A)** near 0 · **(B)** near 1 · **(C)** near 0.5?

- *Answer: C, ~0.60.* The point: the model is *telling you it is unsure*, and it is right to be — mid-grey is genuinely marginal to a human eye too. Then the follow-up worth a minute: most systems collapse that 0.60 to "DARK" and discard the uncertainty. Routing low-confidence cases to a human is a simple, powerful use of a number that is already there and usually ignored.

---

## Discussion prompts (the 15-minute Q&A)

**1. A vendor demos a model that trains live on stage — the accuracy climbs, the room applauds. What have you actually learned about their product?**
- *Surfaces:* essentially nothing. The room just did exactly that in twenty-five minutes with sixteen parameters. A model that trains proves the code runs; it says nothing about data quality, label correctness, the base rate, which errors it makes, or whether it holds up outside the demo. Good answers start listing the questions: what's the held-out score, on what data, what's the class balance, show me the errors. This is the single most job-relevant prompt in the session — open with it if the room is quiet.

**2. Our untrained model scored 55% by answering "DARK" to everything. Where in your work would a "predict the majority class" model score embarrassingly well?**
- *Surfaces:* the base-rate problem, in their own domain. Expect: build-pipeline defects, escaped defects per release, rare hardware faults, security incidents, SLA breaches — all heavily imbalanced, all cases where "nothing will go wrong" is right 95–99.9% of the time. Good answers notice this is *most* of the interesting problems in release and problem management. Directly seeds Session 13.

**3. Why do we hold back a third of the data instead of training on all of it? Isn't that wasting data?**
- *Surfaces:* the difference between memorising and learning, and that the training score is not evidence. Good answers get to "the only score that means anything comes from rows the model has never seen." A great answer goes further: even the *test* set gets contaminated if you keep tuning against it — which is why a three-way split exists, and why the test set should be touched approximately once. Sets up Session 8.

**4. We checked which label meant "dark" before trusting anything. What would have happened if we'd got it backwards — and would we have noticed?**
- *Surfaces:* that a model trained on inverted labels trains *perfectly*: loss falls, accuracy climbs, the log looks healthy, and every prediction is exactly wrong. **Nothing in the training process can detect it.** Only checking the semantics against the world catches it. Generalise: the machinery optimises whatever you point it at, including nonsense, and does so convincingly.

**5. When would you *not* use a neural network?**
- *Surfaces:* the honest heuristic from Session 3 — structured/tabular data with a modest number of features → simpler models first; perceptual, fuzzy, high-dimensional problems (images, audio, language) → neural networks earn their cost. Good answers name our own example as a case in point: brightness is nearly a weighted sum, so a rule or a logistic regression does the job, and the lab's "delete the nonlinearity" challenge demonstrates it. A great answer adds interpretability and audit: a decision tree you can read may beat a more accurate model you cannot (Session 5).

**6. (For the managers.) You are not going to write this code. What did you actually get out of watching it?**
- *Surfaces:* calibration, and a redistribution of scepticism. The build is small and fast; therefore "we built a model" is not an achievement claim, it is a starting line. The management-relevant work is upstream (is the data any good, are the labels right, who decided what the label means) and downstream (which errors does it make, what do they cost, who is accountable). Good answers relocate their questions from the model to the data and the consequences.

**7. Our model outputs 0.98 for cream and 0.60 for mid-grey, and we threw both away by thresholding at 0.5. What could you do with the confidence number instead?**
- *Surfaces:* triage. Auto-decide the confident cases, route the uncertain ones to a human, and you have a system that is both faster *and* safer than either extreme. This is the practical shape of "human in the loop" done well — the human sees the hard cases, not a random sample. Also worth noting: 0.5 is a choice, and moving it is a deliberate trade (Session 8).

**8. (Open / forward-looking.) You now know a working classifier is fifteen lines. Does that make you more or less impressed by the AI products you use?**
- *Surfaces:* the course's central editorial stance, arrived at by the room rather than asserted. Both answers are defensible and the disagreement is the value. "Less impressed" — the modelling is commoditised. "More impressed" — because the room now understands that the visible part is the easy part, and the products that work have solved the hard, invisible parts. Either way, the takeaway is the same: judge AI systems by their data, their errors, and their operating constraints, not by the fact that they exist.

---

## If the room is quiet

Start with **1** (vendor demo) — it converts the lab into something about their job within a single answer. If they are still quiet, go to **2** and ask each person to name one imbalanced dataset from their own work; a round-robin of concrete examples always unsticks a room.

## If the room is fast and technical

Push into the lab's break-it challenges: *why* does removing `/255` hurt but not always catastrophically? *Why* does deleting the nonlinearity barely matter here — and what would a problem look like where it mattered enormously? Those two questions have real depth and reward developers who finished early.
