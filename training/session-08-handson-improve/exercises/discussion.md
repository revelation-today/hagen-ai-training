# Discussion & Polls — Session 8

For the 15-minute Q&A block and in-session polls. Each prompt notes what a good answer surfaces. Prompts are ordered from concrete (about the lab you just ran) to applied (about your actual work).

## Live polls (during the lab)

Run these as quick A/B/C hands or a Colab-cell vote. They keep the room predicting *before* the reveal.

**Poll 1 — after the forced overfit (Part 1).**
> Training accuracy is **1.00**. Is this: **(A)** great news, ship it · **(B)** meaningless on its own · **(C)** actively a warning sign?
- *Best answer: B, leaning C.* 1.00 train accuracy on 60 rows is exactly what memorisation looks like. It's meaningless without the validation number, and a *perfect* train score with imperfect validation is a red flag, not a trophy.

**Poll 2 — before the dropout run (Part 2).**
> After adding dropout, will *training* accuracy go **(A)** up · **(B)** down · **(C)** stay the same?
- *Best answer: B.* Dropout handicaps training on purpose (it's off at evaluation), so train accuracy usually drops while the gap to validation shrinks — the counter-intuitive result worth dwelling on.

**Poll 3 — after Cell 11, before Cell 12 (Part 5).**
> The breast-cancer model reports **96% accuracy**. Is that **(A)** good enough to deploy · **(B)** not enough information · **(C)** clearly too low?
- *Best answer: B.* You can't judge it until you know the base rate and which errors it made. Cell 12 then reveals 3 missed cancers — the reveal lands harder because the room committed to an answer first.

## Discussion prompts (the 15-minute Q&A)

**1. "Training accuracy is 100%." Your junior engineer reports this proudly in standup. What's your next sentence?**
- *Surfaces:* the reflex to ask for the *test/validation* number, not celebrate the training one. Good answers reach for "what did it score on data it hasn't seen?" A great answer notes that 100% train accuracy is more suspicious than reassuring.

**2. For each of these, which error is worse — a false positive or a false negative — and what would you do to the threshold?**
   - a cancer screening model · a spam filter · a fraud-detection flag on a customer's card · a résumé-screening filter
- *Surfaces:* that the answer is *problem-dependent* and about *cost*, not about the model. Cancer/fraud: false negatives worse → lower the threshold, accept more false alarms. Spam: false positive (lost real mail) often worse. Résumé screening: a false negative silently discards a good candidate and is invisible — a fairness landmine. There is no universal "good" threshold.

**3. A vendor says their model is "97% accurate." List the questions you'd ask before believing it means anything.**
- *Surfaces:* the five-question checklist from `content/04` — on which data, what's the base rate, show the confusion matrix, which error costs more, precision/recall on the class that matters. This is the direct rehearsal for Session 13.

**4. Early stopping, dropout, and more data all reduce overfitting. If you could only use one on your next project, which — and what determines the choice?**
- *Surfaces:* that *more data* is the real cure but often unavailable/expensive; *early stopping* is nearly free and should be default; *dropout* is the go-to when data is fixed. Good answers connect the choice to constraints (labelling cost, time, whether the net is oversized).

**5. Why is it cheating to tune your model against the test set until the test score looks good?**
- *Surfaces:* information leakage — every time you react to the test set you bleed a little of it into your choices, slowly overfitting *to the test set itself*. Motivates the three-way split (train / validation / test) and sets up Session 13. Good answers note the test set should be touched approximately once.

**6. The exact same code trained two models: one predicts font colour, one predicts malignancy, both ~96% accurate. Why do we scrutinise one far harder than the other?**
- *Surfaces:* accuracy is identical but the *cost of error* is not — a wrong font is cosmetic, a missed tumour is fatal. The workflow transfers; the judgement about "good enough" does not, and must be re-derived from stakes. This is the thesis of `content/05`.

**7. (For the managers in the room.) You don't write the training code — so what part of this session is actually *your* job?**
- *Surfaces:* that reading and interrogating the *numbers* is a management function, not a coding one. The go/no-go decision, the choice of which error the organisation can tolerate, and the refusal to accept a bare accuracy figure are all management calls. The five questions are a manager's tool.

**8. (Open / forward-looking.) Where in your current work does someone already report a single number that might be hiding its mistakes?**
- *Surfaces:* transfer to the room's real context — defect-detection rates, test-pass percentages, SLA compliance, triage accuracy. Almost every KPI is an "accuracy" that could hide a confusion matrix. Good answers name a specific metric they'll now ask a harder question about.
