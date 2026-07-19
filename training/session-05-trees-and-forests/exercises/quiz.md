# Quiz — Session 5: Decision Trees & Random Forests

Eight self-check questions. Answers at the bottom.

---

**Q1.** In a decision tree, what is a *leaf* node?
- a) The first question asked of every example
- b) A node that makes a prediction (no further question)
- c) A feature that was never used
- d) A branch that was pruned

**Q2.** Compute the Gini impurity of a group with 3 "yes" and 1 "no" (4 examples total).
- a) 0.0
- b) 0.375
- c) 0.5
- d) 0.75

**Q3.** A node is *pure* (all one class). Its Gini impurity is:
- a) 0.0
- b) 0.5
- c) 1.0
- d) depends on the class

**Q4.** A tree scores which split as best?
- a) The one whose children have the **highest** weighted Gini impurity
- b) The one whose children have the **lowest** weighted Gini impurity
- c) The one that uses the most features
- d) A randomly chosen split

**Q5.** You grow a single tree with no depth limit. Training accuracy is 100%, test accuracy is 88%. This is a textbook case of:
- a) Underfitting (high bias)
- b) Overfitting (high variance)
- c) A data leak
- d) A perfectly tuned model

**Q6.** In a bootstrap sample (draw N rows from N, with replacement), roughly what fraction of the original rows are left out of any given tree?
- a) ~10%
- b) ~37%
- c) ~50%
- d) ~63%

**Q7.** What is the *out-of-bag (OOB)* score?
- a) The training accuracy of the forest
- b) An error estimate from a separate holdout set you must create
- c) An error estimate computed by scoring each row with only the trees that did **not** train on it
- d) The accuracy of the single best tree in the forest

**Q8.** Your team must record a written justification for each automated decision in a change-review system. Two models are available: a single decision tree (91% accurate) and a random forest (97% accurate). Which is the better fit, and why?
- a) The forest — always pick the higher accuracy
- b) The tree — it gives a readable, defensible reason for each individual decision
- c) Neither — you must use a neural network for change reviews
- d) The forest — its feature importances explain each individual decision

---

## Answer key

**Q1 — b.** A leaf makes the prediction; internal nodes ask questions, the root is the first question. (`content/01`)

**Q2 — b, 0.375.** `1 − (3/4)² − (1/4)² = 1 − 0.5625 − 0.0625 = 0.375`. (`content/02`)

**Q3 — a, 0.0.** A pure node has one class fraction = 1, so `1 − 1² = 0`. (Note: 0.5 is the *maximally impure* two-class case — the reverse of purity. This corrects a known source-deck error.) (`content/02`)

**Q4 — b.** The tree greedily picks the split giving the **lowest** weighted child impurity — the least-mixed children, i.e. the biggest drop in cost. (`content/02`)

**Q5 — b, overfitting (high variance).** Perfect on training, notably worse on test = memorised the training data, including its noise. A single unconstrained tree is the archetype. (`content/03`)

**Q6 — b, ~37%.** `(1 − 1/N)^N → 1/e ≈ 0.368`. Those left-out rows are the out-of-bag set. (~63% distinct rows *are* used.) (`content/03`)

**Q7 — c.** OOB scores each row using only the trees that never saw it — an unbiased generalisation estimate from the training data, no separate holdout needed. (`content/03`)

**Q8 — b.** When each decision must be justified in writing, a readable tree's exact rule path is a complete, checkable justification. The forest is more accurate but gives only model-level feature importances, not a per-decision reason (so **d** is wrong: importances don't explain an individual decision). This is the session's core trade-off. (`content/04`)

**Scoring:** 7–8 solid; 5–6 re-read `content/02` and `03`; ≤4 re-read the session from `content/00`.
