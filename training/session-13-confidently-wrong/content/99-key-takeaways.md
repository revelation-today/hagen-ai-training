# Key Takeaways — Session 13

---

## The six things

1. **Hallucination has mitigations, not a cure.** Grounding/RAG converts *extrinsic* hallucination (unverifiable invention) into *intrinsic* hallucination (contradicting a source you hold) — checkable, therefore better, and not solved. It adds its own failures: retrieval misses, plausibly-wrong passages, and synthesis across two correct citations into one false claim. Groundedness is measurable (RAGAS, ARES); ask a vendor for a **faithfulness** number, not only an accuracy number.

2. **The 99% trap inverts the usual intuition.** *If it's right 99% of the time, spotting the 1% is harder, not easier.* Three mechanisms: shifted expectation, thinned attention, and — the strongest — the surviving errors are *selected* for being hard to detect, because the obvious ones were fixed first. **Model improvement is not automatically risk reduction.** It re-shapes the risk and quietly weakens the human control that was catching it.

3. **Human-in-the-loop is necessary and not sufficient.** The test: *could this reviewer have produced the correct output themselves, given the same inputs and time?* If not, they are rating prose, and a good model never fails on prose. Check qualification, access to the source of truth, time, and — decisively — whether they are measured on catch rate or on throughput.

4. **Accuracy is an average, and averages annihilate rare events.** The Michael model: 98% accuracy, sensitivity **0**, precision **0**, F1 **undefined**. "Predict nobody ever quits" scores **99%** with no model at all. Always compute the degenerate baseline; always read `support`; prefer `macro avg` over `weighted avg` on imbalanced problems.

5. **Base rates decide everything, and precision is not a property of the model.** Sensitivity and specificity travel with the test; **precision depends on the population you deploy into.** 85% of homicidal offenders being gamers gives P(homicidal | gamer) = **0.02%**. A vendor's 99% sensitivity and 79.8% precision, measured where 20% were at risk, becomes **13.8%** precision where 1% are — six of every seven positives are false alarms. *Never associate a common trait with an uncommon one.*

6. **The number was probably found under pressure.** P-hacking is *not usually malicious — human nature under pressure and career survival*: **"No paper, no funding." "Our client wants to see 10% savings." "Our VC investors want a demonstration."** Defend with process, not suspicion: a validation set touched once, pre-registration, all runs reported, and your own data before you buy.

---

## The numbers to remember

| Number | What it is |
|---|---|
| **98%** | Accuracy of a model that predicts only people named Michael will quit — and gets both interesting cases wrong |
| **99%** | Accuracy of predicting nobody ever quits, using no model at all |
| **0, 0, undefined** | Michael's sensitivity, precision, F1 |
| **0.02%** | P(homicidal offender \| plays violent video games), from a study finding 85% the other way |
| **99% → 79.8% → 3.39%** | The vendor reveal, as the source deck stages it |
| **13.8%** | The correctly-worked figure. Same verdict: don't buy it |
| **619 / 718** | False alarms per quarter from an AI defect-detection tool at a 1% base rate |

---

## The artefact to take away

> ### Four questions for any AI accuracy claim
> 1. **That percentage — of what population?** Sensitivity, precision, or accuracy?
> 2. **What is the precision, on what test set?** Full confusion matrix, raw counts.
> 3. **What was the base rate of the positive class in your test set?**
> 4. **What is the base rate in *our* population?** ← **you must answer this one yourself.**
>
> Then apply Bayes at *your* base rate, and ask: **what happens when it is wrong, and who finds out?**

---

## If you remember one thing

> **A number can be completely true and completely misleading, and the gap between the two is the population it was measured in. Ask what the base rate was — then go and find out what yours is.**

---

## Where this goes next

- **Session 14 — Risk II:** the failures that come from *how you deploy it*. Prompt injection, data leakage, the hazard triangle, the operating domain, EU AI Act. Session 13 assumed nobody was attacking you. Session 14 removes that assumption.
- **Session 15 — Limits and jobs:** the S-curve and the proof-of-concept-to-production gap. The economic version of everything here — *it is not AI capability that grows exponentially, it is the cost of closing the last gap.*
