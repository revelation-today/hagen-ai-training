# Lab — Clustering & Anomaly Detection with scikit-learn (~25 min)

A hands-on clustering lab you can finish in about 25 minutes. You'll run **K-means** with the **elbow method**, run **DBSCAN**, watch DBSCAN separate shapes K-means can't, and turn it into a tiny **anomaly detector**. No labels, no maths beyond what Session 4 covered.

## Setup (2 min)

**Colab-first (recommended):**
1. Go to [colab.research.google.com](https://colab.research.google.com) → **New notebook** (Google account required).
2. scikit-learn, numpy, and matplotlib are pre-installed. Nothing to `pip install`.

**JupyterLite fallback (no account, fully in-browser):**
1. Open [jupyter.org/try-jupyter/lab/](https://jupyter.org/try-jupyter/lab/) → new **Python (Pyodide)** notebook.
2. If an import is missing, run `import piplite; await piplite.install("scikit-learn")` in the first cell. Everything else works the same.

> Both run in the browser with no local install. Colab is faster; JupyterLite needs no login and sidesteps notebook-hosting policy questions.

---

## Step 1 — Make some data and scale it (3 min)

```python
import numpy as np
from sklearn.datasets import make_blobs
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans, DBSCAN
from sklearn.metrics import silhouette_score
import matplotlib.pyplot as plt

# 3 blobs of "normal" data — pretend we don't know there are 3.
X, _ = make_blobs(n_samples=300, centers=3, cluster_std=0.70, random_state=42)
X = StandardScaler().fit_transform(X)     # ALWAYS scale before a distance-based method

plt.scatter(X[:, 0], X[:, 1], s=12); plt.title("raw data — how many clusters?"); plt.show()
```
**Expected:** a scatter plot with three visually separable blobs.

---

## Step 2 — The elbow method: how many clusters? (5 min)

```python
sse = []
for k in range(1, 9):
    km = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X)
    sse.append(km.inertia_)            # inertia = within-cluster SSE

print("SSE by K:", [round(s) for s in sse])
# SSE by K: [600, 260, 47, 39, 33, 28, 24, 21]

plt.plot(range(1, 9), sse, "o-"); plt.xlabel("K"); plt.ylabel("SSE (inertia)")
plt.title("Elbow method"); plt.show()
```
**Expected:** SSE drops steeply to K=3, then flattens. The **bend at K=3** is the elbow. Note the trap: SSE keeps falling forever, so you can't just take the minimum.

---

## Step 3 — Silhouette cross-check (3 min)

```python
for k in range(2, 7):
    km = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X)
    print(f"K={k}  silhouette={silhouette_score(X, km.labels_):.3f}")
# K=2  silhouette=0.581
# K=3  silhouette=0.840   <-- highest => confirms the elbow
# K=4  silhouette=0.663
# K=5  silhouette=0.552
# K=6  silhouette=0.470
```
**Expected:** silhouette peaks at K=3 (≈0.84), agreeing with the elbow. Two independent signals → confident K=3.

---

## Step 4 — Fit K-means and see the clusters (3 min)

```python
km = KMeans(n_clusters=3, n_init=10, random_state=42).fit(X)
plt.scatter(X[:, 0], X[:, 1], c=km.labels_, s=12, cmap="viridis")
plt.scatter(*km.cluster_centers_.T, c="red", marker="X", s=200)  # centroids
plt.title("K-means, K=3"); plt.show()
print("cluster sizes:", np.bincount(km.labels_))   # cluster sizes: [100 100 100]
```
**Expected:** three coloured clusters, red X centroids at their centres, ~100 points each.

---

## Step 5 — DBSCAN on shapes K-means gets wrong (5 min)

```python
from sklearn.datasets import make_moons
Xm, _ = make_moons(n_samples=300, noise=0.06, random_state=42)
Xm = StandardScaler().fit_transform(Xm)

# K-means slices both crescents in half:
km2 = KMeans(n_clusters=2, n_init=10, random_state=42).fit(Xm)
# DBSCAN follows the density and traces each crescent + flags noise:
db = DBSCAN(eps=0.20, min_samples=5).fit(Xm)

n_clusters = len(set(db.labels_)) - (1 if -1 in db.labels_ else 0)
print("DBSCAN clusters:", n_clusters, "| noise points:", list(db.labels_).count(-1))
# DBSCAN clusters: 2 | noise points: 7

fig, ax = plt.subplots(1, 2, figsize=(10, 4))
ax[0].scatter(Xm[:,0], Xm[:,1], c=km2.labels_, s=12); ax[0].set_title("K-means (wrong)")
ax[1].scatter(Xm[:,0], Xm[:,1], c=db.labels_, s=12);  ax[1].set_title("DBSCAN (right; -1=noise)")
plt.show()
```
**Expected:** K-means cuts a straight line through both crescents; DBSCAN colours each crescent separately and marks ~7 stray points as noise (label −1). DBSCAN discovered "2" without being told.

---

## Step 6 — A tiny anomaly detector (4 min)

```python
# Inject 5 oddball configs into the normal blobs, then let DBSCAN flag them.
rng = np.random.RandomState(0)
anomalies = rng.uniform(-10, 10, size=(5, 2))
Xa = StandardScaler().fit_transform(np.vstack([make_blobs(300, centers=3, cluster_std=0.6,
                                                          random_state=42)[0], anomalies]))
db = DBSCAN(eps=0.35, min_samples=5).fit(Xa)
flagged = np.where(db.labels_ == -1)[0]
print("flagged as anomalies (label -1):", flagged)
# the last 5 indices (300..304) — our injected oddballs — appear in the flagged list
print("all 5 injected caught:", set(range(300, 305)).issubset(set(flagged)))
# all 5 injected caught: True
```
**Expected:** DBSCAN's `-1` label catches the injected anomalies — no labelled "bad" examples were used. That's the anomaly-detection pattern from `content/04`.

---

## Now break it / now extend it

1. **Break the elbow:** set `cluster_std=3.0` in Step 1's `make_blobs` so the blobs overlap. Re-run Steps 2–3. Does the elbow still bend cleanly? Does silhouette still peak at 3? (Lesson: when clusters overlap, the "right K" gets genuinely ambiguous — the methods stop being decisive, and that's honest information.)
2. **Break DBSCAN:** in Step 5 change `eps` to `0.05`, then `0.6`. Watch it flip between "everything is noise" and "one giant cluster." (Lesson: `eps` is the sensitive knob — tune it with a k-distance plot, don't guess.)
3. **Extend to PCA:** load `from sklearn.datasets import load_digits` (64-D), run `PCA(n_components=2)` then scatter the result coloured by the true digit label. Then try `TSNE(n_components=2)` on the same data. Which separates the digits more clearly? (Lesson: t-SNE to see — but remember not to trust the between-cluster distances.)

**If a cell errors in JupyterLite:** run `import piplite; await piplite.install("scikit-learn")` once, then re-run. Colab needs none of this.
