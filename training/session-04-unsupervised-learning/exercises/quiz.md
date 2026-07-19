# Quiz — Session 4: Unsupervised Learning

8 self-check questions. Answers at the bottom. No peeking.

---

**Q1.** What single thing distinguishes unsupervised from supervised learning?

**Q2.** Put the K-means loop steps in order: (a) recentre, (b) assign to nearest centroid, (c) initialise K centroids, (d) repeat until no point changes cluster.

**Q3.** Why can't you choose K by simply picking the K with the lowest SSE (inertia)?

**Q4.** A silhouette score of −0.4 for a point tells you what?

**Q5.** In DBSCAN, what does a label of `-1` mean, and why is that label the key to anomaly detection?

**Q6.** Name the two DBSCAN parameters and, in one line each, what they control.

**Q7.** True or false: on a t-SNE plot, two clusters drawn far apart are definitely more different than two drawn close together. Explain.

**Q8.** Complete the rule of thumb and justify it in one sentence: "t-SNE to ____, PCA to ____."

**Bonus.** You have 200-column config records and want to cluster them, but the clustering looks like noise. What's the standard first fix, and why?

---

## Answer key

**A1.** Unsupervised learning has **no labels/answers** — only features. It discovers structure (clusters, reduced dimensions) rather than predicting a known target, so there's no ordinary accuracy score.

**A2.** **c → b → a → d**: initialise K centroids → assign each point to nearest → recentre to the mean → repeat until assignments stop changing (convergence).

**A3.** Because **SSE always decreases as K increases** — at K = number of points, every point is its own centroid and SSE = 0. Minimising SSE would pick a useless K. You take the **elbow** (where the curve bends from steep to flat) instead, and cross-check with silhouette.

**A4.** The point is likely **in the wrong cluster** — it's closer, on average, to a neighbouring cluster than to its own. (Silhouette runs −1 to +1: ≈+1 well-placed, ≈0 on a boundary, <0 misassigned.)

**A5.** `-1` marks a **noise point** — one that is neither a core point nor within `eps` of a core point, so it belongs to no cluster. It's the anomaly because it doesn't resemble any dense "normal" region; DBSCAN gives you outlier detection for free, with no labelled bad examples.

**A6.** **`eps`** — the neighbourhood radius (how close counts as "close"); **`min_samples` (MinPts)** — how many points must be within `eps` for a region to count as dense (making a point a core point).

**A7.** **False.** t-SNE preserves *local* neighbourhoods, not *global* geometry — between-cluster distances (and cluster sizes) on the plot are not reliable. Treat the picture as a hypothesis, then confirm with a metric-bearing method (clustering + silhouette).

**A8.** "t-SNE to **see**, PCA to **compute**." t-SNE/UMAP make a 2-D picture that reveals local structure (visualisation); PCA is linear, fast, and reversible, so it's used to reduce/denoise data before feeding another model or clustering it.

**Bonus.** Run **PCA first** (reduce 200 columns to, say, 20–30 components), *then* cluster the result. In high dimensions the **curse of dimensionality** makes every point roughly equidistant, so distance-based clustering breaks; PCA restores meaningful distance and removes noise. (Also verify you **scaled** the features — an unscaled large-unit column can dominate everything.)
