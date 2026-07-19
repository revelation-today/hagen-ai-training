# Sources — Session 4: Unsupervised Learning

Every source used, with licence status and a one-line reuse verdict. **SLIDE-SAFE** = permissive / CC-BY / BSD / MIT / public domain — may derive slides and figures *with attribution*. **LINK-ONLY** = all-rights-reserved / NC / ND — assign as reading or run as a live demo, **never copy onto a slide**. Verdicts verified 2026-07-18 (see the research memo behind this session).

> **Provenance note.** This session's original coverage of unsupervised learning existed **only** in the excluded `Cisco Confidential` deck (*Mastering the Fundamentals of AI and ML*, Barton & Henry — see `../../../AI_input.md` §1). That deck is **LINK-ONLY at best and excluded in practice**; none of its text or figures appear here. The session is rebuilt entirely from the SLIDE-SAFE sources below. The topic (K-means, DBSCAN, PCA, t-SNE/UMAP, anomaly detection) is textbook-standard and fully served by open sources.

---

## SLIDE-SAFE — build slides and figures from these (with attribution)

**1. scikit-learn — User Guide: Clustering.** scikit-learn developers. https://scikit-learn.org/stable/modules/clustering.html — HTML prose + generated figures. **Licence: BSD-3-Clause → SLIDE-SAFE.** The scikit-learn FAQ confirms both the documentation prose and the generated figures are usable under BSD-3 (only the logo is excluded). **Primary source** for K-means, DBSCAN, SSE/inertia, silhouette. Used in `content/00`, `01`, `02`, `04`.

**2. scikit-learn — User Guide: Decomposition (PCA).** https://scikit-learn.org/stable/modules/decomposition.html — **BSD-3 → SLIDE-SAFE.** Source for PCA, explained-variance ratio, components-for-target-variance. Used in `content/03`.

**3. Distill — "How to Use t-SNE Effectively."** Wattenberg, Viégas & Johnson, Distill, 2016. https://distill.pub/2016/misread-tsne/ — interactive article. **Licence: CC-BY 2.0 → SLIDE-SAFE** (footer: "Diagrams and text are licensed under Creative Commons Attribution CC-BY 2.0 … source on GitHub"; skip the few figures it marks as reused from elsewhere). Source for the t-SNE traps (cluster sizes / distances / perplexity). Used in `content/03`, slide 17.

**4. PAIR — "Understanding UMAP."** Coenen & Pearce, Google PAIR, 2019. https://pair-code.github.io/understanding-umap/ — interactive article. **Licence: Apache-2.0** (repo `github.com/PAIR-code/understanding-umap`) **→ SLIDE-SAFE.** Source for UMAP and the t-SNE-vs-UMAP comparison. Used in `content/03`, slide 18.

**5. setosa.io — "Principal Component Analysis explained visually."** Victor Powell / setosa.io. https://setosa.io/ev/principal-component-analysis/ — interactive. **Licence: MIT** (repo `github.com/vicapow/explained-visually`) **→ SLIDE-SAFE** (may embed/screenshot). Best live/embeddable PCA visual. Slide 15.

**6. scikit-learn — Example: "Selecting the number of clusters with silhouette analysis on KMeans."** https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html — **BSD-3 → SLIDE-SAFE.** Reusable code + figure. Basis for the silhouette cross-check in `content/01` and the lab.

**7. scikit-learn — Example: "Demo of DBSCAN clustering algorithm."** https://scikit-learn.org/stable/auto_examples/cluster/plot_dbscan.html — **BSD-3 → SLIDE-SAFE.** Reusable code + figure. Basis for `content/02` and slide 13.

**8. scikit-learn — Manifold learning (t-SNE) + Novelty/Outlier detection.** https://scikit-learn.org/stable/modules/manifold.html and https://scikit-learn.org/stable/modules/outlier_detection.html — **BSD-3 → SLIDE-SAFE.** t-SNE usage and the anomaly-detection framing / further-reading pointers (Isolation Forest, LOF) in `content/03`, `04`.

**9. Google — Clustering course (ML Education).** https://developers.google.com/machine-learning/clustering/ — **Licence: content CC-BY, code Apache-2.0 → SLIDE-SAFE** with attribution to Google. Optional supplementary framing for clustering quality. Not heavily drawn on; listed for completeness.

**10. Aurélien Géron — `handson-ml3` notebooks** (`09_unsupervised_learning.ipynb`, `08_dimensionality_reduction.ipynb`). https://github.com/ageron/handson-ml3 — **Licence: Apache-2.0 → SLIDE-SAFE.** Colab-ready reference notebooks; optional extended lab material.

---

## LINK-ONLY — reference, assign, or demo live; never copy onto a slide

**11. Naftali Harris — "Visualizing K-Means Clustering."** https://www.naftaliharris.com/blog/visualizing-k-means-clustering/ — step-through, pick centroids. **No stated licence → LINK-ONLY.** Perfect **live demo** for the K-means loop (slide 6). Do not screenshot into slides.

**12. Naftali Harris — "Visualizing DBSCAN Clustering."** https://www.naftaliharris.com/blog/visualizing-dbscan-clustering/ — drag `eps`/MinPts, watch core/border/noise. **No stated licence → LINK-ONLY.** Live demo for slide 11. Do not screenshot.

**13. StatQuest (Josh Starmer)** — YouTube: K-means, PCA, t-SNE, DBSCAN. https://www.youtube.com/c/joshstarmer — **All-rights-reserved → LINK-ONLY.** The StatQuest FAQ explicitly asks people **not to use screenshots**. Excellent **assigned pre-watch**; never embed frames.

**14. r2d3 — "A Visual Introduction to Machine Learning."** Yee & Chu. https://r2d3.us/visual-intro-to-machine-learning-part-1/ — **No open licence; companion dataset is CC-BY-NC-SA → LINK-ONLY.** Great intuition builder for a live demo / assigned reading (more relevant to Session 5, referenced here for clustering intuition). Do not copy visuals.

**15. `Mastering the Fundamentals of AI and ML`** — Barton & Henry, Cisco, 2025. **`Cisco Confidential` → EXCLUDED** (not merely LINK-ONLY). Listed only to record that the original coverage of this topic came from here and was deliberately not used. See `../../../AI_input.md` §1.

---

## Further reading (the best LINK-ONLY material — assign, don't slide)

- **StatQuest** clustering & dimensionality-reduction playlists (#13) — the friendliest video intros; assign before the session.
- **Naftali Harris** K-means (#11) and DBSCAN (#12) visualisers — the single best way to *feel* how each algorithm moves; run live or send as a link.
- **r2d3** visual intro (#14) — beautiful scroll-driven ML intuition.
- **scikit-learn** clustering & decomposition user guides (#1, #2) — also the definitive *reference* once you want depth beyond the slides (these are SLIDE-SAFE too, so no restriction — just longer than a slide).
- For anomaly detection beyond this session: scikit-learn's **Isolation Forest** and **Local Outlier Factor** (BSD-3, SLIDE-SAFE) — purpose-built detectors, a natural next step from the three methods in `content/04`.

---

## Attribution string for slides

Footer tags to use on derived slides (per `../../powerpoint_instructions.md` §3):
- scikit-learn figures/prose → **"scikit-learn, BSD-3"**
- Distill t-SNE → **"Distill (Wattenberg et al.), CC BY 2.0"**
- PAIR UMAP → **"Google PAIR, Apache-2.0"**
- setosa.io PCA → **"setosa.io, MIT"**
