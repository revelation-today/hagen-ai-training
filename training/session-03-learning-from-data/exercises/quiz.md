# Quiz — Session 3

Eight self-check questions plus a bonus. Try them before looking at the answer key at the bottom. Aim to *explain*, not just recognise — most of these are questions you would actually ask a team, so a one-word answer is not a passing answer.

---

**Q1.** Complete both arrows and name the inversion. Traditional software: `rules + data → ______`. Supervised learning: `data + ______ → ______`.

**Q2.** In the running colour example, name the **features** and the **label**, and say which one a person had to produce by hand. What is the sharpest question to ask about that hand-produced part?

**Q3.** A vendor reports **99.4% accuracy**. Give the two questions you ask before the number means anything, and explain what a "100% accurate" model most likely indicates.

**Q4.** Why three sets rather than two? Explain what specifically leaks if you use the test set to choose between five candidate models, and state the golden rule about the test set in one sentence.

**Q5.** Match each set to what it determines:

| Set | Determines… |
|---|---|
| Training | ? |
| Validation | ? |
| Test | ? |

Choose from: *the hyperparameters (the settings about the model)*, *nothing — it only judges*, *the parameters (the settings inside the model)*.

**Q6.** Classify each as **regression** or **classification**:
   (a) How many days until this disk fails?
   (b) Will this disk fail within 30 days?
   (c) Which team should this support ticket be routed to?
   (d) How many incidents will we see next quarter?

**Q7.** A model outputs **P(risky) = 0.62** for a configuration change, and the system blocks the change. Explain the full mechanism that turned 0.62 into "blocked", and say who should own the number in the middle and on what grounds.

**Q8.** For each problem, apply the decision heuristic and justify in one line:
   (a) Predict build duration from a table of repo size, module count, and machine type.
   (b) Decide whether a photo of a device screen shows a cracked panel.
   (c) Flag anomalous logins from a table of counts, times, and locations.

**Q9 (bonus).** In the lab, the fully-grown tree scored **1.000** on training data and **0.847** on test data, while a depth-4 tree scored **0.889** on training and **0.876** on test. Which model do you ship, and what does the comparison prove about the relationship between training accuracy and quality?

---

## Answer key

**A1.** Traditional software: `rules + data → **answers**` — a human writes the rules. Supervised learning: `data + **answers** → **rules**` — you supply worked examples (inputs *with* their correct outputs) and the machine infers the rule. The inversion is that the answers become an *input* to the process and the rules become the *output*. (`content/00`, `content/01`)

**A2.** **Features:** the three colour channels — Red, Green, Blue, each 0–255. **Label:** whether that background needs a **light** or **dark** font. The **label** is the hand-produced part: a person (or a hand-written rule) decided it for all 1,345 rows. The sharpest question is **"where did the labels come from, and how consistent were they?"** — a supervised model's quality is capped by its labels, and no modelling cleverness repairs sloppy or biased labelling. (`content/01`)

**A3.** Ask: (1) **On which data — training or held-out?** and (2) **How many times did you look at the test set while tuning?** A reported **100%** almost always means the model is being graded on its own homework: it memorised the training set, noise included. Perfect training accuracy is evidence of overfitting, not of quality. (`content/03`; lab cell 3, where the unlimited tree scores exactly 1.000 on training and 0.847 on unseen data)

**A4.** With only train and test, you inevitably use the test set to *choose* between candidates — and the moment a test score influences a choice, your choices have been fitted to the test set. It is no longer unseen, and the final score becomes optimistic: a slower version of the same memorisation trap. The **validation** set absorbs all comparing and tuning so the test set stays sealed. **Golden rule: the test set is touched exactly once, at the very end — any test result that influences a decision contaminates it.** (`content/03`)

**A5.**

| Set | Determines… |
|---|---|
| **Training** | the **parameters** — the settings *inside* the model (weights, biases, split thresholds) |
| **Validation** | the **hyperparameters** — the settings *about* the model (which algorithm, how deep, when to stop, which threshold) |
| **Test** | **nothing — it only judges.** It is uninvolved in both, which is what makes its number trustworthy. |

(`content/03`; demonstrated in lab cells 4 and 5, where validation picks `max_depth=4` and test reports 0.876.)

**A6.** (a) **Regression** — a number on a continuous scale. (b) **Classification** — yes/no. (c) **Classification** — a category from a fixed set of teams. (d) **Regression** — a count on a scale. Note (a) and (b): the same underlying concern gives a different problem *shape* depending on whether the label is a number or a choice, and the classification version is often more useful because it ends in a decision. (`content/04`)

**A7.** The model does not output "risky". It outputs a **probability** — 0.62 — which is a regression onto the 0–1 scale. A **threshold** (here, some cut-off at or below 0.62) is then compared against it by a dead-simple rule: `if P(risky) ≥ threshold, block`. That threshold is a **business choice, not a technical constant**: lowering it catches more genuinely risky changes at the cost of more needless reviews; raising it does the reverse. It should be owned by whoever bears the two costs — a change board or service owner weighing "a bad change ships" against "a human wastes ten minutes" — **not** by the model, and not silently by whoever left the default at 0.5. (`content/04`)

**A8.** (a) **Simple model** (linear/tree regression) — structured tabular data, features already meaningful; cheaper, faster, auditable. (b) **Neural network** — perceptual data (raw pixels); nobody can hand-write the rule for "cracked panel", so the model must learn its own features. (c) **Simple model first** — tabular counts and times; escalate only if simple methods honestly fail to separate the anomalies. Overall: *structured → simple, perceptual → neural, use the simplest model that works.* (`content/05`)

**A9 (bonus).** **Ship the depth-4 tree.** It is worse on training data (0.889 vs 1.000) and better on the data that matters (0.876 vs 0.847). The comparison proves that training accuracy and quality are not merely imperfectly correlated — past the good-fit point they move in **opposite** directions, because extra capacity is spent memorising noise (the lab deliberately corrupts 8% of labels). Hence: judge a model by its score on data it has never seen, and prefer the simplest model that clears the bar. (`content/03`, `content/05`, `content/99`; lab cells 3–5)
