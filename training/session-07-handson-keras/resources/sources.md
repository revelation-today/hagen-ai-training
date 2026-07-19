# Sources & Licences — Session 7

Reuse verdicts govern what may go on a slide. **SLIDE-SAFE** = permissive / BSD / Apache / CC-BY / public-domain — may derive slides and figures with attribution. **LINK-ONLY** = all-rights-reserved / NC / ND — reference it, assign it, or demo it live; never copy it onto a slide. When in doubt, LINK-ONLY.

---

## Primary source material (the topic origin)

**1. Deep Learning for Beginners — Day 1** — Thomas Nield, O'Reilly Media, 2024-08 (78 slides).
- Supplies the *idea* of this session: the light/dark-font colour problem as a running example, the `3 → 3 → 1` architecture, and the TensorFlow/Keras build (its pp. 38 and 40).
- **Licence:** commercial O'Reilly training deck, all rights reserved.
- **Verdict: LINK-ONLY.** Nothing is reproduced. Critically, that deck's **two exercise slides (p. 41, p. 78) contain only the words "EXERCISE 1" and "EXERCISE 2"** — no prompt, no starter code, no solution (`AI_input.md` §0, item 2). **The entire lab in `exercises/lab.md` is therefore authored from scratch**, as is every diagram, table and checkpoint in this folder.

### Source defects corrected in this session, not repeated

| Ref | Defect in the source | What we do instead | Where |
|---|---|---|---|
| `AI_input.md` §6 **#1** | **Threshold contradiction:** p. 26 states output ≥ .5 → **DARK**; p. 35 states ≥ .5 → **light**. The two slides disagree. | We hold **≥ 0.5 → DARK** throughout (the output is defined as the probability of *dark* text) — consistently across Sessions 6, 7 and 8. The lab adds a cell that **verifies empirically** that the dataset's labels point that way, because resolving it in prose is worthless if the data disagrees. | `content/02`, `content/07`, `lab.md` Cell 3, Slide 3 |
| `AI_input.md` §6 **#2** | **Initialisation claim:** weights stated as initialised "between −1 and 1" (pp. 30, 49), but the accompanying code uses `np.random.rand` (p. 77), which returns **0 to 1** — all positive. Both cannot be true. | We repeat neither claim. The lab **prints Keras's actual initial weights** and names the real default: **Glorot-uniform** (symmetric around zero, range scaled to layer width) with **zero biases**. We also explain why the all-positive version would be materially worse. | `content/03`, `lab.md` Cell 5 |
| `AI_input.md` §5 | **Loss choice:** the source compiles a sigmoid classifier with `MeanSquaredError` — unconventional; cross-entropy is expected. Likely a simplification to keep one loss across a three-day course. | We use **`binary_crossentropy`**, explain why (stronger gradients when confidently wrong), and flag the change. Session 8 uses the same. | `content/04` |
| `AI_input.md` §6 **#13** | The source defines "deep learning" as more than one hidden layer, but its own running example has **one** — so the course's own network is not deep learning. | Called out explicitly rather than hidden, in both the content and the lab. | `content/03`, `content/00` |
| `AI_input.md` §6 **#3** | `confusion_matrix` imported but never used (p. 40). | Not carried over. The confusion matrix is taught properly in **Session 8**, where it has a purpose. | — |

## Slide-safe technical sources (may be embedded, with attribution)

**2. TensorFlow / Keras** — `Sequential`, `Input`, `Dense`, `relu`, `sigmoid`, `compile`, `fit`, `evaluate`, `predict`, `get_weights`, the Adam optimizer, `binary_crossentropy`, and the Glorot-uniform default initialiser.
- URL: https://keras.io · https://www.tensorflow.org · initialiser reference: https://keras.io/api/layers/initializers/
- **Licence: Apache-2.0.** **Verdict: SLIDE-SAFE** — the API surface and our code using it may be shown with attribution. Code slides carry the footer tag *"Keras API, Apache-2.0"*.

**3. scikit-learn** — `train_test_split` (with `stratify` and `random_state`).
- URL: https://scikit-learn.org
- **Licence: BSD-3-Clause.** **Verdict: SLIDE-SAFE** with attribution.

**4. The colour dataset** (`light_dark_font_training_set.csv` — RGB triples + a binary light/dark-font label).
- Short link used in the lab: **`https://tinyurl.com/y2qmhfsr`**
- Resolves to: `https://raw.githubusercontent.com/thomasnield/machine-learning-demo-data/master/classification/light_dark_font_training_set.csv`
- Schema confirmed: `RED,GREEN,BLUE,LIGHT_OR_DARK_FONT_IND`.
- **Status:** confirmed resolving **2026-07-17** (`research_labs_evals_gov.md` §A2 — all three of the source course's 2024 short-links still resolve). ⚠️ **VERIFY IT RESOLVES ON THE MORNING OF DELIVERY.** It is a 2024-era short link on a third-party shortener, pointing at a repo we do not control; either could disappear without notice.
- **Row count caveat:** the source deck claims 1,345 rows; the research pass estimated fewer. We do not state a row count as fact anywhere — the lab prints the actual number and the material says "~1,300".
- The repo carries **no explicit licence**. The content is factual numeric data (RGB triples plus a derived label), which is not itself creatively copyrightable, and we use it only as lab input loaded at runtime.
- **Verdict: usable as lab data (link, load at runtime); treat as LINK-ONLY for any slide reproduction** — never put dataset content on a slide.
- **Mitigation — the lab does not depend on it.** `exercises/lab.md` **Cell 1b** synthesises an equivalent dataset in six lines of NumPy (random RGB, labelled by the standard perceived-luminance formula `0.299R + 0.587G + 0.114B`). The lab then runs identically and reaches comparable accuracy. This removes the session's only network dependency.

**5. Our own lab code, prose, tables, checkpoints and Mermaid diagrams** (this session folder).
- **Verdict: SLIDE-SAFE** (internal Qualcomm material). All numeric outputs shown are marked **illustrative** — training is stochastic and results vary between runs even with seeds set.

## Environment / tooling

**6. Google Colab** — the primary lab environment (free tier; CPU is sufficient — the model has 16 parameters and needs no GPU).
- URL: https://colab.research.google.com · FAQ (primary): https://research.google.com/colaboratory/faq.html
- **Verdict:** tool/link only; the interface may be screenshotted. Vendor service — free-tier limits are deliberately unpublished and drift over time (`research_labs_evals_gov.md` §A1).

**7. JupyterLite** — **not usable for this session.**
- URL: https://jupyterlite.readthedocs.io/
- Runs Python entirely in the browser via WebAssembly with **no account and no server** — the lowest-friction option in the course, and self-hostable on an intranet page. NumPy, pandas, matplotlib and scikit-learn work.
- **But there is no WebAssembly build of TensorFlow/Keras**, so none of Session 7 can run there (`research_labs_evals_gov.md` §A1). This is not a configuration problem solvable on the day. Anyone blocked from Colab should pair with a colleague or use Kaggle Notebooks.
- **Verdict:** tool/link only.

**8. Kaggle Notebooks** — the backup environment (free, TensorFlow pre-installed, most generous free GPU quota — which we do not need).
- URL: https://www.kaggle.com/code · GPU quota (primary): https://www.kaggle.com/docs/efficient-gpu-usage
- **Verdict:** tool/link only. Requires an account, and phone verification can be an obstacle for corporate sign-ups.

## Research provenance (session-internal)

**9. `extract_dl1.md`** — the full page-by-page extraction of *Deep Learning for Beginners* Day 1, including the verbatim TensorFlow build (p. 38), the train/test-split code (p. 40), the activation-function selection table (p. 32), and the three internal inconsistencies flagged for correction.

**10. `research_labs_evals_gov.md`** (compiled 2026-07-17) — Part A: the lab-environment comparison (Colab / Kaggle / Codespaces / Deepnote / JupyterLite), the JupyterLite Keras limitation, and the 2026-07-17 confirmation that `tinyurl.com/y2qmhfsr` resolves to a valid CSV with the expected schema.

**11. `AI_input.md`** — the consolidated corpus extract. §5 (Currency Register — the MSE-with-sigmoid note, the dead Katacoda labs) and §6 (Error Register — defects #1, #2, #3, #13) drive the corrections applied above. §0 item 2 documents that all exercise prompts in the corpus are missing and must be authored.

---

## Further reading (LINK-ONLY — assign, don't re-slide)

- **Keras — "The Sequential model" guide** (Apache-2.0, actually slide-safe, but best assigned as hands-on reading): https://keras.io/guides/sequential_model/
- **Keras — Dense layer API reference** (Apache-2.0): https://keras.io/api/layers/core_layers/dense/
- **3Blue1Brown — *Neural Networks*, chapters 1–4** — the best visual intuition for what `fit()` is doing under the hood. **All rights reserved: LINK-ONLY**, assign as pre- or post-reading, never re-slide: https://www.youtube.com/watch?v=aircAruvnKk
- **Thomas Nield, *Deep Learning for Beginners* (O'Reilly live training)** — the origin of this session's running example, for anyone who wants the from-scratch NumPy version we deliberately skipped per the Keras-level steer. **LINK-ONLY.**
- **`thomasnield/machine-learning-demo-data`** — the demo-data repo behind the colour dataset; confirmed live 2026-07-17: https://github.com/thomasnield/machine-learning-demo-data
