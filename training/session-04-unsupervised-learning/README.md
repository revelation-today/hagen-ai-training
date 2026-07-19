# Session 4 — Methods II: Unsupervised Learning

**Block:** Methods (know the four families) · **Goal covered:** 3 (Methods explained) · **Format:** 45 min content + 15 min Q&A

---

## Summary

Sessions 3 and this one split machine learning by what the data gives you. Session 3 covered **supervised** learning — you have labelled answers and the model learns to reproduce them. This session covers **unsupervised** learning — you have *no* labels, only raw data, and the job is to let the structure in the data speak for itself. Two things you can do without labels: **group similar things together** (clustering) and **squeeze many measurements into a few that still carry the signal** (dimensionality reduction). The pay-off for a release / problem / configuration-management audience is concrete: cluster your "normal" configurations or incidents, and the point that refuses to join any cluster is your **anomaly** — a drifted config, a novel incident, a machine behaving unlike its peers. That is a problem/config-management tool, not an abstraction.

## Audience & level

Qualcomm release / problem / configuration managers and developers, with some prior AI exposure. Technically literate; not everyone codes daily. This session assumes Session 3's vocabulary (features, a model, train vs. inference, why you scale data) and builds four named methods on top of it. The code is scikit-learn and is explained line by line; you can follow the ideas without running it, but the optional lab lets you run it in ~25 minutes.

## Learning objectives

By the end, a participant can:

1. **Explain** the difference between supervised and unsupervised learning, and name the two unsupervised jobs (clustering, dimensionality reduction).
2. **Trace** the K-means loop by hand (initialise centroids → assign → recentre → repeat) and use the **elbow method** and **silhouette score** to choose K.
3. **Contrast** K-means and DBSCAN, and decide which to reach for given cluster shape, noise, and whether you know the number of groups in advance.
4. **Distinguish** PCA from t-SNE/UMAP, and apply the rule of thumb *"t-SNE to see, PCA to compute."*
5. **Design** a simple anomaly detector for configs or incidents by clustering the "normal" and flagging what does not fit.
6. **Choose** the right unsupervised technique for a given task using a decision table.

## Prerequisites

- **Session 2** — vocabulary: model, feature, training vs. inference.
- **Session 3** — supervised learning, the train/test idea, feature scaling and *why* it matters (it matters a lot here — every method in this session is distance-based).
- No calculus. No prior clustering experience assumed.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–4 min | **Hook** | "You have 10,000 server configs and no labels. Which one is about to page you at 3 a.m.?" Frame unsupervised learning as *structure without answers*. |
| 4–8 min | **The map** | Supervised vs. unsupervised; the two jobs (cluster / reduce); where the four named methods sit. |
| 8–20 min | **Clustering: K-means** | The loop, live on a 2-D scatter; SSE + the elbow; silhouette. The three ways it bites you (pick K, spherical assumption, scaling/init). |
| 20–30 min | **Clustering: DBSCAN** | Density instead of centroids; core/border/noise; finds odd shapes and *labels the noise* — which is the anomaly hook. Live density visualiser demo. |
| 30–39 min | **Dimensionality reduction** | PCA (directions that matter, explained variance) vs. t-SNE/UMAP (see the high-dimensional shape). "t-SNE to see, PCA to compute." The trap: don't over-read a t-SNE picture. |
| 39–45 min | **The landing: anomaly detection** | Cluster "normal", flag the outlier. A config/incident-management tool. The "which technique when" decision table. |
| 45–60 min | **Q&A** | Discussion prompts from `exercises/discussion.md`. |

Honest note on timing: this is four methods in 45 minutes. It is deliberately paced as *intuition + one worked example each*, not exhaustive coverage. Depth lives in the `content/` files; the deck stays sparse. If time is tight, DBSCAN's parameter-tuning detail and UMAP are the first things to cut to the reading.

## Materials & tools

- **Slides:** `slides/outline.md` (built per `../powerpoint_instructions.md`).
- **Reading:** `content/00`–`content/99` — the full self-study material.
- **Runnable demo:** in `content/01`, `content/02`, and `content/04` — scikit-learn KMeans + DBSCAN on a small 2-D dataset with the elbow method. Expected output shown in comments.
- **Lab:** `exercises/lab.md` — a ~25-minute clustering lab, **Google Colab first**, JupyterLite fallback (fully in-browser, no account).
- **Live demos (link-only — never embedded on a slide):** Naftali Harris's [K-means](https://www.naftaliharris.com/blog/visualizing-k-means-clustering/) and [DBSCAN](https://www.naftaliharris.com/blog/visualizing-dbscan-clustering/) visualisers; StatQuest videos as pre-watch.

## Source & licence note

This session's original coverage lived **only** in a `Cisco Confidential` deck, which is **excluded** (see `../AI_input.md` §1). It is rebuilt from clean, licence-checked public material. The reuse verdicts are governed by `resources/sources.md`:

- **SLIDE-SAFE (build slides + figures from these):** scikit-learn User Guide & examples gallery (BSD-3 — prose *and* generated figures reusable), Distill "How to Use t-SNE Effectively" (CC-BY 2.0), PAIR "Understanding UMAP" (Apache-2.0), setosa.io PCA visual (MIT).
- **LINK-ONLY (assign as reading / run live — never copy onto a slide):** StatQuest videos, r2d3 visual intro, the Naftali Harris visualisers, and the excluded Cisco deck.

The house voice applies: name what each method *cannot* do, distinguish a pretty demo from a production detector, and prefer worked numbers over adjectives.
