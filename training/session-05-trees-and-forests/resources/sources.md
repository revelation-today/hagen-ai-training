# Sources & Licences — Session 5: Decision Trees & Random Forests

This topic existed in the source corpus **only** in the excluded `Cisco Confidential` deck (see `../../AI_input.md` §1 and `../../training_proposal.md` §2). The session is therefore **rebuilt entirely from clean public sources**. Reuse verdicts: **SLIDE-SAFE** = may derive slides/prose/figures with attribution; **LINK-ONLY** = reference, assign as reading, or show as a live demo, never copy onto a slide. When in doubt, LINK-ONLY.

Licence status verified 2026-07-18 against the research brief in the session scratchpad (`research_classical_ml.md`).

---

## Primary source (build slides from this)

**1. scikit-learn — User Guide & Examples Gallery (Decision Trees + Ensembles/Forests)**
- Org: scikit-learn developers. URLs: <https://scikit-learn.org/stable/modules/tree.html>, <https://scikit-learn.org/stable/modules/ensemble.html#forest>, and gallery examples `plot_forest_importances.html`, `plot_ensemble_oob.html`, `plot_tree_regression.html`.
- Version: scikit-learn 1.5.x (current 2026). Format: HTML prose + generated figures + downloadable notebooks.
- **Licence: BSD-3-Clause. Verdict: SLIDE-SAFE.** The scikit-learn FAQ explicitly confirms that both the documentation prose and the images generated within the docs may be reused under BSD-3 (only the project *logo* is excluded). This is the source for Gini, bootstrap, bagging, OOB error, majority voting, feature importances, and any `plot_tree` / importance figure put on a slide.
- Used for: all of `content/02`–`content/05`, the demo (`content/05`, `exercises/lab.md`), and every derived slide figure (tag footer `scikit-learn, BSD-3`).

**2. `sklearn.datasets.load_breast_cancer` (Breast Cancer Wisconsin Diagnostic)**
- Bundled with scikit-learn (BSD-3 distribution); originally the UCI ML Repository dataset (Wolberg, Street & Mangasarian), public for research/education.
- **Verdict: SLIDE-SAFE** for use in the demo/lab. Used only as a neutral, built-in tabular example to show overfitting, OOB, and importances — no medical claims are made.
- Used for: `content/05` Parts 2–5, `exercises/lab.md` Steps 2–5.

## Standard reference (the worked example)

**3. "Buys a computer?" (`AllElectronics`) dataset — Han, Kamber & Pei, *Data Mining: Concepts and Techniques***
- The 14-row age/income/student/credit → buys example is the canonical decision-tree teaching dataset, reproduced across countless courses and textbooks. Here it is re-rendered as a small original table and all Gini figures are computed from scratch.
- **Verdict:** the dataset (14 rows of synthetic attributes) is a de-facto public teaching example; our table and computations are original. Safe to use with attribution to the standard textbook example. The book text/figures themselves are copyrighted — **do not** copy the book's prose or diagrams; we reproduce only the raw data table and our own maths.
- Used for: `content/01`, `content/02`, `content/05` Part 1, `exercises/lab.md` Step 1.

## Corpus provenance

**4. Excluded Cisco deck — "Mastering the Fundamentals of AI and ML," Barton & Henry (`Cisco Confidential`)**
- The only corpus coverage of trees/forests (`AI_input.md` §2.4, §6). **EXCLUDED** — confidentiality-marked third-party internal material; not usable to train Qualcomm. **Not cited, not derived from.** Its "cost/distance" pedagogical spine is a standard idea we re-derive independently from scikit-learn and re-frame in our own words; its **Gini transcription error** (printing 0.5 for a pure split, `AI_input.md` §6 #11) is explicitly *corrected* in `content/02` and the quiz, not repeated.
- Verdict: **DO NOT USE.**

---

## Further reading & live demos (LINK-ONLY — never embed)

**5. r2d3.us — "A Visual Introduction to Machine Learning," Parts 1 & 2** — Stephanie Yee & Tony Chu. <https://r2d3.us/visual-intro-to-machine-learning-part-1/>
- The best intuition-builder for decision trees, overfitting, and bias/variance (scroll-driven interactive). Verified live 2026-07-18.
- **Licence: no open licence; the associated dataset is CC-BY-NC-SA. Verdict: LINK-ONLY.** Assign as pre-reading or run as a live demo (Slide 18). **Do not copy its visuals onto a slide.**

**6. StatQuest with Josh Starmer — "Decision Trees," "Random Forests Part 1 & 2"** — YouTube. <https://www.youtube.com/c/joshstarmer>
- Excellent plain-language video explanations of Gini, bagging, and OOB.
- **Licence: all-rights-reserved; the channel explicitly asks people not to use screenshots. Verdict: LINK-ONLY.** Assign as pre-watch video; never put frames on a slide.

**7. Géron, *Hands-On Machine Learning* — `handson-ml3` notebooks** — `06_decision_trees.ipynb`, `07_ensemble_learning_and_random_forests.ipynb`. <https://github.com/ageron/handson-ml3>
- **Licence: Apache-2.0 (code/notebooks). Verdict: SLIDE-SAFE** for code reuse. Optional deeper lab / instructor reference; our lab is self-contained on scikit-learn, so this is supplementary.

---

## Quick verdict table

| # | Source | Licence | Verdict |
|---|---|---|---|
| 1 | scikit-learn user guide + gallery (trees, forests, OOB, importances) | BSD-3-Clause | **SLIDE-SAFE** |
| 2 | `load_breast_cancer` (bundled dataset) | BSD-3 dist. / UCI public | **SLIDE-SAFE** (demo) |
| 3 | "Buys a computer?" 14-row example (our table + our maths) | standard teaching data; original rendering | **SLIDE-SAFE** (attribute the example) |
| 4 | Cisco deck (trees/forests) | Cisco Confidential | **DO NOT USE** |
| 5 | r2d3.us visual intro | no open licence / CC-BY-NC-SA data | **LINK-ONLY** |
| 6 | StatQuest videos | all-rights-reserved | **LINK-ONLY** |
| 7 | Géron `handson-ml3` notebooks | Apache-2.0 | **SLIDE-SAFE** (code) |
