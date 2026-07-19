# Quiz — Session 8

Ten self-check questions. Answers and explanations at the bottom. Try them before scrolling.

## Questions

**1.** Your model scores 0.99 accuracy on the training set and 0.71 on the test set. What is this, and what is the single most likely first fix if you *cannot* get more data?

**2.** A different model scores 0.68 on training and 0.67 on test. Is this overfitting? What is it, and what do you change?

**3.** In a plot of accuracy vs. epoch, what specific visual pattern tells you overfitting has begun?

**4.** You add `Dropout(0.5)` and notice training accuracy is now *lower* than validation accuracy. Is something broken? Explain.

**5.** What does `restore_best_weights=True` do in `EarlyStopping`, and why is leaving it at the default (`False`) a common mistake?

**6.** Your loss becomes `NaN` after a few epochs. What is the most likely cause and the first knob to change?

**7.** Fill in the confusion matrix cells. Positive = "fraud." The model flagged 100 transactions as fraud; 30 of those were actually fraud. There were 50 actual frauds in total. Give TP, FP, FN, and then precision and recall.

**8.** A model predicting a disease that affects 2% of people reports 98% accuracy. Why is this unimpressive, and what would you ask to see?

**9.** For a cancer-screening model, you move the decision threshold from 0.50 down to 0.30. What happens to recall and precision on the malignant class, and why might you want this?

**10.** You train the *identical* code on the colour dataset and the breast-cancer dataset; both hit ~96% accuracy. Why do you interrogate the second model far more than the first?

---

## Answer key

**1.** **Overfitting** (high train, much lower test — a ~0.28 gap). With no new data available, the strongest first moves are **early stopping** (nearly free, turn it on) and/or **dropout** / **shrinking the network** to reduce capacity. More data would be best but is ruled out here. *(content/01, 02)*

**2.** **Not overfitting** — the two scores are close, so there's no memorisation gap. This is **underfitting** (both low): the model is too simple or under-trained. Fix by **adding capacity** (bigger/deeper network), **training longer**, or **raising the learning rate** if it's too small. *(content/01, 03)*

**3.** The **train and validation lines start together, then split apart** — training accuracy keeps climbing while validation flattens (or turns down). That divergence is overfitting. Validation *loss* often shows it earlier, forming a "U" whose minimum is the best model. *(content/01)*

**4.** **Nothing is broken — this is dropout working correctly.** Dropout randomly disables neurons *during training only*; it is switched off during evaluation. So the model is handicapped when computing training accuracy but runs at full strength for validation, which can push validation above training. It's a sign the regularisation is active. *(content/02)*

**5.** It **rolls the model back to the weights from the best epoch** (lowest validation loss) rather than keeping the last epoch's weights. Left at `False`, you stop training late but keep the *worse*, already-overfitting final weights — you get the timing benefit but not the quality benefit. Always set it to `True`. *(content/02)*

**6.** The **learning rate is too high** — steps overshoot and the optimiser diverges. First fix: **divide the learning rate by 3–10** (e.g. `1e-2 → 1e-3`). It's the most common training failure. *(content/03)*

**7.** Flagged 100 as fraud, 30 correct → **TP = 30, FP = 70**. Total actual frauds = 50, of which 30 were caught → **FN = 20**. **Precision = 30/100 = 0.30** (only 30% of flags were real fraud). **Recall = 30/50 = 0.60** (caught 60% of actual fraud). A noisy detector that still misses 40% of fraud. *(content/04)*

**8.** Because a model that simply **predicts "no disease" for everyone scores 98%** — equal to the base rate — while catching **zero** cases. Accuracy is inflated by the common class. Ask for the **confusion matrix** and **recall/precision on the disease class**; a useful model must beat the trivial 98%-by-doing-nothing baseline on *recall*. *(content/04)*

**9.** Lowering the threshold flags more cases as malignant → **recall goes up** (you miss fewer real cancers) and **precision goes down** (more false alarms / unnecessary follow-ups). You want this when a **false negative (missed cancer) is far costlier than a false positive (extra biopsy)** — so you deliberately trade precision for recall. *(content/04)*

**10.** Because **accuracy is identical but the cost of error is not.** A wrong font colour is cosmetic; a missed malignancy (a false negative hidden inside that 96%) can be fatal. The workflow transfers unchanged, but the judgement "is 96% good enough?" must be re-derived from the stakes — and only the confusion matrix, not the accuracy, reveals the missed cases. *(content/05, 04)*

---

**Score guide:** 8–10 you're ready to interrogate any model's numbers · 5–7 re-read `content/04` · below 5 re-run the lab, watching the scoreboard and the confusion matrix.
