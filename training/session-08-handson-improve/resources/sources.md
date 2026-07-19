# Sources & Licences — Session 8

Reuse verdicts govern what may go on a slide. **SLIDE-SAFE** = permissive / BSD / Apache / CC-BY / public-domain — may derive slides/figures with attribution. **LINK-ONLY** = all-rights-reserved / NC / ND — reference, assign, or demo live; never copy onto a slide. When in doubt, LINK-ONLY.

---

## Primary source material (the topic origin)

**1. Deep Learning for Beginners — Day 3, §Testing & Validation** — Thomas Nield, O'Reilly Media, 2024-09.
- Provides the topic set: overfitting, bias–variance, confusion matrix, precision/recall/specificity/F1, ROC/AUC, and the "Michael" accuracy parable.
- **Licence:** commercial O'Reilly training deck, all rights reserved.
- **Verdict: LINK-ONLY.** Nothing is copied. The "Michael" parable is *paraphrased with our own framing*; every code cell is newly written by us. The source's dead **Katacoda** labs are replaced with fresh Colab code. Two source defects are corrected in our materials: (a) it uses **mean-squared-error loss with a sigmoid classifier** — we use the conventional **`binary_crossentropy`** (see `AI_input.md` §5); (b) Katacoda retired 2022 — links dead (`AI_input.md` §5 Currency Register).

## Slide-safe technical sources (may be embedded, with attribution)

**2. scikit-learn** — `sklearn.metrics` (`confusion_matrix`, `classification_report`, `precision_score`, `recall_score`, `f1_score`, `ConfusionMatrixDisplay`, `RocCurveDisplay`), `StandardScaler`, `train_test_split`, and the **bundled Breast Cancer Wisconsin (Diagnostic) dataset** (`load_breast_cancer`).
- URL: https://scikit-learn.org · dataset: https://scikit-learn.org/stable/datasets/toy_dataset.html#breast-cancer-dataset
- **Licence: BSD-3-Clause.** **Verdict: SLIDE-SAFE** — prose, API, figures, and the dataset may be used with attribution. The breast-cancer data originates from the UCI ML Repository (Wolberg et al., public research dataset) and ships inside the library, so no download and no network dependency.

**3. TensorFlow / Keras** — `Sequential`, `Dense`, `Dropout`, `Adam`, `EarlyStopping`, `fit`, `evaluate`, `predict`.
- URL: https://keras.io · https://www.tensorflow.org
- **Licence: Apache-2.0.** **Verdict: SLIDE-SAFE** — API surface and our code using it may be shown with attribution.

**4. The colour dataset** (`light_dark_font_training_set.csv`, RGB → light/dark-font label) — Thomas Nield's `machine-learning-demo-data` GitHub repo (the Session 7 spine dataset).
- URL: https://raw.githubusercontent.com/thomasnield/machine-learning-demo-data/master/classification/light_dark_font_training_set.csv
- **Status:** confirmed live 2026-07-17 (via `research_labs_evals_gov.md` §A2 — the `tinyurl.com/y2qmhfsr` short-link resolves here; schema `RED,GREEN,BLUE,LIGHT_OR_DARK_FONT_IND` verified). The repo carries **no explicit licence**. The content is **factual numeric data** (RGB triples + a derived label), which is not itself creatively copyrightable, and we use it only as lab input, not as slide content.
- **Verdict: usable as lab data (link, load at runtime); treat as LINK-ONLY for any slide reproduction.** If the link ever dies, the identical data is trivially regenerable (RGB values + a luminance rule) or replaceable with any scikit-learn toy dataset — the lab does not depend on this specific file.

**5. Our own lab code, prose, tables, and Mermaid diagrams** (this session folder).
- **Verdict: SLIDE-SAFE** (internal Qualcomm material). All illustrative output numbers are marked as such — they will vary with random initialisation.

## Environment / tooling

**6. Google Colab** — primary lab environment (free tier; CPU sufficient for these tiny models).
- URL: https://colab.research.google.com · FAQ: https://research.google.com/colaboratory/faq.html
- **Verdict:** tool/link only (SLIDE-SAFE to link and screenshot the interface). Vendor service — free-tier limits are unpublished and drift (`research_labs_evals_gov.md` §A1).

**7. JupyterLite** — *partial* fallback only.
- URL: https://jupyterlite.readthedocs.io/
- **Note:** JupyterLite runs scikit-learn in-browser (no login, no server) and can do **Part 5** (`sklearn.metrics`), but **cannot run TensorFlow/Keras** (no WASM build) — so the Keras Parts 1–4 must be watched, not run, if Colab is blocked. Verdict: tool/link only. (`research_labs_evals_gov.md` §A1.)

## Research provenance (session-internal)

**8. `research_labs_evals_gov.md`** (compiled 2026-07-17) — Part A: lab-environment comparison (Colab vs. Kaggle vs. JupyterLite), the JupyterLite Keras limitation, and the 2026-07-17 confirmation that the colour-dataset short-link still resolves.

**9. `AI_input.md`** — the consolidated corpus extract. §5 (Currency Register: dead Katacoda, MSE-vs-cross-entropy) and §6 (Error Register) drive the two corrections we apply; §2.1 supplies the Day 3 asset ranking (Michael parable, confusion-matrix worked example).

---

## Further reading (LINK-ONLY — assign, don't re-slide)

- **scikit-learn — "Confusion matrix" and "Precision-Recall" user-guide examples** (BSD-3, actually slide-safe, but excellent as hands-on reading): https://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html · https://scikit-learn.org/stable/auto_examples/model_selection/plot_precision_recall.html
- **Google — "Classification: Precision and Recall" (ML Crash Course)** — clear explainer; Google content, treat as **LINK-ONLY** (assign as pre-reading, don't copy): https://developers.google.com/machine-learning/crash-course/classification/precision-and-recall
- **3Blue1Brown — neural-network series** (overfitting/generalisation intuition) — **LINK-ONLY** (all rights reserved): https://www.youtube.com/watch?v=aircAruvnKk
- **Andrew Ng, "the proof-of-concept-to-production gap"** (IEEE Spectrum interview) — quotable *concept*, paraphrase only; **LINK-ONLY**. Reinforces why "doing well on a test set is not the job." (`extract_dl3.md` p.71.)
