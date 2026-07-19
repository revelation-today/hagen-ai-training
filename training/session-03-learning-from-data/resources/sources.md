# Sources — Session 3

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / BSD / public-domain / standards body. May derive slide and content text/figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NC / ND / vendor / internal. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY.

**Summary for this session:** exactly one slide in the deck (Slide 12, the code demo) carries embedded derived material, and it derives from **scikit-learn (BSD-3) — SLIDE-SAFE**. Everything conceptual is either **authored fresh for this course** or **paraphrased in our own words from a LINK-ONLY internal source deck**. No O'Reilly deck content — text, figures, or slide layouts — is reproduced anywhere in this session.

| # | Source | Licence | Verdict | Used for |
|---|---|---|---|---|
| 1 | scikit-learn documentation & library | BSD-3-Clause | **SLIDE-SAFE** | All code: `train_test_split`, `DecisionTreeClassifier`, `.score`, `predict_proba` |
| 2 | NumPy documentation & library | BSD-3-Clause | **SLIDE-SAFE** | Dataset generation in the lab |
| 3 | *Deep Learning for Beginners — Day 1* (O'Reilly, internal source deck) | All rights reserved | **LINK-ONLY** | Conceptual framing, paraphrased only |
| 4 | Authored-fresh course material | n/a — ours | **SLIDE-SAFE** | The 70/15/15 three-way split treatment, all Mermaid diagrams, all worked judgements, the lab |

---

## Slide-safe (embeddable with attribution)

**#1 — scikit-learn (Pedregosa et al. / the scikit-learn developers).**
- URL: `https://scikit-learn.org/stable/` · `train_test_split`: `https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html` · `DecisionTreeClassifier`: `https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html` · cross-validation guide: `https://scikit-learn.org/stable/modules/cross_validation.html`
- Version: 1.x (any current release; the API used here is long-stable).
- Licence: **BSD-3-Clause** — permissive; use, modification, and redistribution permitted with the copyright notice. Documentation prose is likewise BSD-3-licensed.
- Used for: the split code and the train-vs-test score comparison in `content/03-train-val-test-split.md`; the whole of `exercises/lab.md`; Slide 12.
- **Reuse verdict: SLIDE-SAFE.** Put the code on a slide with the footer tag **"scikit-learn, BSD-3-Clause"**.
- *Note:* the code snippets in this session are our own composition against a public API, not copied from the docs — the licence permits either, but authorship is ours.

**#2 — NumPy (Harris et al. / the NumPy developers).**
- URL: `https://numpy.org/doc/stable/`
- Licence: **BSD-3-Clause**. **Reuse verdict: SLIDE-SAFE.**
- Used for: `default_rng` dataset generation in `exercises/lab.md` cell 1. Generating the data locally is deliberate — it keeps the lab reproducible, dependency-light, and free of any third-party dataset licence.

**#4 — Course-authored material (this project).**
- Licence: authored fresh for this course; no third-party constraint.
- Covers: the **70 / 15 / 15 three-way split** treatment and the parameters/hyperparameters mapping (the source deck used a two-way 2/3–1/3 split with no validation set; we add the validation third and explain the leak it prevents — see `content/03`); **every Mermaid diagram** in this session; the **worked judgements table** in `content/05`; the **cost asymmetry** treatment in `content/02`; the **8%-label-noise lab** and all its numbers; the **manager's four questions** in `content/99`.
- **Reuse verdict: SLIDE-SAFE.** Free to render, redraw, and re-palette in the deck.
- *Provenance note:* the 70/15/15 split, the underfit/good-fit/overfit framing, the memorised-exam analogy, and k-fold cross-validation are **standard textbook practice** with no single owner. They are stated here in our own words and carry no licence encumbrance.

---

## Link-only (reference / assign / paraphrase — never embed)

**#3 — Nield, T. *Deep Learning for Beginners — Day 1, §I* (O'Reilly live training).** Internal source deck for this course.
- Licence: **all rights reserved** (O'Reilly live-training material), held internally. **LINK-ONLY** — internal reference; do not reproduce slides, figures, wording, or layout, and do not circulate the deck outside the training team.
- Used for (all **paraphrased into our own words**, never copied):
  - the **`data + answers → rules`** inversion that opens the session (`content/00`, Slide 3);
  - the **light/dark font colour problem** as a running example — our own table of RGB rows, not their slide (`content/01`, Slide 4);
  - the **dark-clouds-mean-rain** intuition for what a model is (`content/02`, Slide 6);
  - **regression vs. classification**, and the sigmoid-output-then-threshold mechanism (`content/04`);
  - the **when-is-a-neural-network-justified** heuristic and the **"use the simplest model that works"** discipline note, including the author's own admission that the toy problem "would probably be better solved with logistic regression" (`content/05`, Slides 17–18).
- *Handling:* attribute the *framing* verbally in the room ("this framing follows an O'Reilly deep-learning course we use internally"); assign the deck as optional follow-up reading to anyone with access. Nothing from it reaches a slide.

### Corrections carried into this session (per `../../../AI_input.md` §6)

| Source-deck issue | What we do instead |
|---|---|
| **Error #1 — the light/dark threshold contradiction.** One slide states the output is P(**dark**) with ≥ 0.5 → dark; a later slide states ≥ 0.5 → **light**. Both cannot be true. | We fix a single convention and never flip it: the output is **P(dark font)**; threshold 0.5; **≥ 0.5 → dark**, **< 0.5 → light**. The contradiction is named on Slide 16 as a credibility beat, with the general lesson: *always pin down what the probability is the probability of.* (`content/04`) |
| **Two-way 2/3–1/3 split, no validation set.** Sufficient for a teaching demo, but it leaves the tuning leak unaddressed. | We teach the **three-way 70/15/15** split and explain explicitly *why* the third set exists (`content/03`). We say plainly that the percentages are convention, and give the exceptions: huge datasets, tiny datasets (k-fold cross-validation), and time-series (split by time, never randomly). |

Neither correction is presented as a criticism of the source in the room beyond Slide 16 — the point is the general habit, not the deck.

---

## Further reading (LINK-ONLY, high quality — assign, don't slide)

| Topic | Suggestion | Why |
|---|---|---|
| The split, done properly, with code | scikit-learn *Cross-validation: evaluating estimator performance* — `https://scikit-learn.org/stable/modules/cross_validation.html` | The one link to give anyone who asks "but what if my dataset is tiny?" Covers k-fold, stratification, and time-series splits. Also the only item here that is itself SLIDE-SAFE. |
| Underfitting vs. overfitting, visually | scikit-learn *Underfitting vs. Overfitting* example — `https://scikit-learn.org/stable/auto_examples/model_selection/plot_underfitting_overfitting.html` | The curve from `content/03` as a runnable plot; a good five-minute follow-up to the lab. |
| The whole picture, textbook-grade | Hastie, Tibshirani & Friedman, *The Elements of Statistical Learning* (free PDF from the authors) | Chapter 7 is the canonical treatment of model assessment and selection. Deep water, but the reference to cite when someone wants rigour behind "hold data back". |
| Practical ML with the same tools | Géron, *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow* (O'Reilly) | All-rights-reserved — assign, never copy. Chapters 1–2 are the natural next step after this session, and it uses the same library the lab uses. |
| Where the framing came from | *Deep Learning for Beginners — Day 1* (#3, internal) | For anyone with O'Reilly access who wants the original treatment of the heuristic. Read it with `content/04`'s correction in hand. |
| Why accuracy alone misleads | Held for **Session 13** (base rates, precision/recall, the confusion matrix) | Deliberately out of scope here — this session's lab has near-balanced classes (47.9% / 52.1%), which is exactly the condition that makes plain accuracy fair. Say so if asked; don't open it early. |
