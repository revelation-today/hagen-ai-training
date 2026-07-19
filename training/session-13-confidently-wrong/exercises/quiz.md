# Quiz — Session 13

Ten self-check questions. Try them before looking at the answer key. Aim to *explain*, not just recognise — several of these are questions you will be asked to answer out loud in a real meeting.

---

**Q1.** A model predicts that only employees named Michael will quit. In a company of 100, one Michael stays and one other person quits. Fill in the confusion matrix (TP, FP, FN, TN) and compute sensitivity, precision, accuracy and F1.

**Q2.** Your colleague says: "F1 came back as 0.00, so the model scored badly — we should retune it." What might actually be going on, and what would you check first?

**Q3.** A vendor says: *"Our tool identifies 99% of unauthorised configuration changes."* Which metric is that, and why can it not, on its own, tell you whether the tool is any good?

**Q4.** A test has 99% sensitivity and 93.75% specificity. It was validated on a sample where 20% had the condition, giving 79.8% precision. Your population has a 1% base rate. Without a calculator, is the precision you will experience higher, lower, or about the same — and roughly by how much?

**Q5.** Explain, in one sentence each, why **sensitivity** and **specificity** travel with a test from population to population, but **precision** does not.

**Q6.** A study finds 85% of homicidal offenders played violent video games. Roughly 19% of the population plays them, and about 0.005% of the population is a homicidal offender. Estimate P(homicidal | gamer) by counting people out of 100,000. Then state the general rule this illustrates.

**Q7.** The same data supports both "gamers are 4× more likely" and "gamers are 24× more likely." Explain how both can be true, and say what a relative risk must always be accompanied by.

**Q8.** True or false: *"A model upgrade from 97% to 99.5% accuracy strictly reduces the risk of a bad output reaching production."* Justify your answer with reference to the mechanism, not just the residual error rate.

**Q9.** Your team proposes: "An engineer reviews every AI-generated change summary before it goes out." Name three questions that determine whether this is a real control or theatre, and give the single sharpest test.

**Q10.** Your analyst tried eleven feature combinations before finding one that hit p < 0.05, and reported that one honestly and in full detail. Is this fraud? What is it, what causes it, and what one cheap control would have caught it?

**Bonus.** A vendor supplies a full confusion matrix, raw counts, precision, recall and F1 — everything you asked for on slides 1–3 of this session's four questions. What is the one thing still missing, and why can only you supply it?

---

## Answer key

**A1.** Convention: positive class = "quits". The Michael was predicted to quit and stayed → **FP = 1**. Someone else quit and was predicted to stay → **FN = 1**. The other 98 were predicted to stay and stayed → **TN = 98**. Nobody who quit was flagged → **TP = 0**.

- Sensitivity = TP/(TP+FN) = 0/1 = **0**
- Precision = TP/(TP+FP) = 0/1 = **0**
- Accuracy = (0+98)/100 = **0.98**
- F1 = 2·(0·0)/(0+0) = **0/0, undefined**

The model failed entirely at the only predictions anyone wanted and still reports 98% accuracy. (file 03)

**A2.** F1 = 0.00 in a report can mean two very different things. It may mean the model scored badly — or it may mean F1 is **mathematically undefined** (0/0, because precision and recall are both zero) and the library rendered it as 0.0. scikit-learn does exactly this, emitting an `UndefinedMetricWarning` that most pipelines suppress. **Check TP first.** If TP = 0, the model made zero correct positive predictions, and possibly zero positive predictions at all — that is a structural failure, not a tuning problem. Also check `support` for the positive class: if it is tiny, no metric on that row means anything. (file 03)

**A3.** That is **sensitivity** — P(flagged | truly unauthorised), measured only on the changes that really were unauthorised. It is blind to how many *authorised* changes also get flagged, so it says nothing about what a flag means when you receive one. Decisively: **a tool that flags every single change has 100% sensitivity.** Since sensitivity alone can always be made perfect, sensitivity alone is never evidence. Ask for precision, the raw confusion matrix, and the base rate of the positive class in their test set. (files 03, 05)

**A4.** **Much lower — roughly six times lower, around 13–14%.** Reasoning without arithmetic: the number of true positives scales with prevalence and drops by a factor of 20 (20% → 1%), while false positives scale with the *healthy* population, which barely changes (80% → 99%, slightly *up*). So the ratio of true to false positives collapses by roughly a factor of 20. Precise answer: **13.8%** — six of every seven positives are false alarms. (file 05)

**A5.** **Sensitivity** is measured only on cases that have the condition, so it does not depend on how many cases don't — it is a property of how the test behaves on positives. **Specificity** is measured only on cases that don't, so it does not depend on how many do. **Precision** mixes the two groups — TP/(TP+FP) has true positives in the numerator and false positives from the *other* group in the denominator — so its value depends on the relative sizes of those groups, i.e. on the prevalence. Precision is a joint property of the test *and the population*. (files 03, 05)

**A6.** Out of 100,000 people: 5 are homicidal offenders (0.005%); 85% of those 5 = **4.25** are gamers; 19% of 100,000 = **19,000** are gamers. So P(homicidal | gamer) = 4.25 / 19,000 = **0.000224 ≈ 0.02%**. From 85% to 0.02% — a four-order-of-magnitude reversal, with nobody disputing the study. The rule: **never take a common trait and associate it with an uncommon one.** The reversal is violent precisely because the denominators (5 vs. 19,000) differ by a factor of ~3,800. (file 04)

**A7.** They use different comparison groups. P(homicidal | gamer) = 0.0224%; P(homicidal | **non**-gamer) = 0.00093% → ratio ≈ **24×**. P(homicidal) across the **whole population** = 0.005% → ratio ≈ **4.5×**. The whole-population figure includes gamers, which dilutes the contrast. Both are honest statements about identical data; the speaker chooses. **A relative risk must always be accompanied by an absolute risk.** "24× more likely" here means 2 in 10,000 instead of 1 in 100,000 — the multiplier is enormous and the risk is negligible, with no contradiction. Always ask: *multiplied from what, compared to whom?* (file 04)

**A8.** **False.** Residual model error does fall — but the human control that catches it degrades at the same time, through three mechanisms: (1) the reviewer's expectation shifts to "usually right," turning review into confirmation; (2) attention thins as throughput expands to consume the accuracy gain; and (3) — the strongest — **the surviving errors are selected for being hard to detect**, because the obvious ones were fixed first. The remaining 0.5% is not a random sample of the old 3%; it is the subtlest part of it. So a model upgrade **invalidates the risk assessment** and the control must be re-derived. Treat the model version as a configuration item. (file 02)

**A9.** Any three of: *Is the reviewer qualified in the subject matter?* · *Do they have the source of truth available at review time, or only the output?* · *Do they have the time — items per day × minutes per item ≤ a working day?* · *Is there any mechanism that would reveal a reviewer approving everything unread?* · *Are they measured on catch rate or on throughput?* (The last one decides the others: a reviewer measured on turnaround time becomes a rubber stamp, rationally and without any bad intent — and you built that.)

The sharpest single test: **could this reviewer have produced the correct output themselves, given the same inputs and enough time?** If not, they can only judge plausibility — and plausibility is exactly the dimension on which a good model never fails. They are rating prose, not verifying facts. (file 02)

**A10.** Not fraud — this is **p-hacking**, and *it is usually not malicious; it is human nature operating under pressure and career survival*. Each of the eleven attempts was individually defensible, and the final report is honest about what it did; what is missing is the eleven that failed, and nobody ever writes those down. The cause is incentive: *"No paper, no funding" · "Our client wants to see 10% savings" · "Our VC investors want a demonstration."* With eleven attempts at a 0.05 threshold, the effective false-positive risk is far above 5%.

Cheapest control: **a held-out validation set, touched exactly once**, after the analysis is fixed. Equally cheap alternatives: pre-register the metric and decision rule before looking at results; report all runs (mean and spread over many seeds and folds) rather than the best; or simply ask *"how many things did you try?"* Treat the validation set as a controlled artefact — versioned, access-restricted, with every use logged. A team that cannot say how many times it has evaluated against its validation set does not have one. (file 06)

**Bonus.** **The base rate of the positive class in *your* population** — question 4 of the four questions. Everything the vendor supplied describes their test set. Sensitivity and specificity transfer; precision does not, and it is precision you will live with. Only you can measure your own prevalence, from your own data, and **you should never accept the vendor's estimate of your world** — they have neither the data nor the incentive. Then recompute precision at your base rate and ask the last question: *what happens when it's wrong, and who finds out?* (file 05)
