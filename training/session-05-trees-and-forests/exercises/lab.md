# Lab — Trees & Forests in scikit-learn (~20–30 min)

A hands-on notebook that reproduces the session's worked example, then builds a random forest and reads its OOB score and feature importances. You will *see* a single tree overfit and a forest fix it. Everything uses **scikit-learn (BSD-3)**.

## Setup

**Colab (recommended):** open <https://colab.research.google.com> → New notebook. scikit-learn, pandas, numpy, and matplotlib are pre-installed — nothing to `pip install`.

**JupyterLite fallback (no account, runs in-browser):** open <https://jupyter.org/try-jupyter/lab/> → new notebook. If a library is missing, run `%pip install scikit-learn pandas matplotlib` in the first cell.

**Local fallback:** any Python 3.10+ with `pip install scikit-learn pandas matplotlib`.

Sanity check:

```python
import sklearn, pandas, numpy, matplotlib
print(sklearn.__version__)   # e.g. 1.5.x  (any recent 1.x is fine)
```

---

## Step 1 — Rebuild the "buys a computer?" tree (~6 min)

Confirm with code what you computed by hand: the root Gini is 0.4592 and the tree splits on **age** first.

```python
import pandas as pd
from sklearn.tree import DecisionTreeClassifier, export_text

rows = [
    ("youth","high","no","fair","no"),        ("youth","high","no","excellent","no"),
    ("middle_aged","high","no","fair","yes"), ("senior","medium","no","fair","yes"),
    ("senior","low","yes","fair","yes"),      ("senior","low","yes","excellent","no"),
    ("middle_aged","low","yes","excellent","yes"), ("youth","medium","no","fair","no"),
    ("youth","low","yes","fair","yes"),       ("senior","medium","yes","fair","yes"),
    ("youth","medium","yes","excellent","yes"),("middle_aged","medium","no","excellent","yes"),
    ("middle_aged","high","yes","fair","yes"),("senior","medium","no","excellent","no"),
]
df = pd.DataFrame(rows, columns=["age","income","student","credit_rating","buys"])

maps = {
    "age":{"youth":0,"middle_aged":1,"senior":2}, "income":{"low":0,"medium":1,"high":2},
    "student":{"no":0,"yes":1}, "credit_rating":{"fair":0,"excellent":1}, "buys":{"no":0,"yes":1},
}
enc = df.replace(maps)
X, y = enc[["age","income","student","credit_rating"]], enc["buys"]

clf = DecisionTreeClassifier(criterion="gini", random_state=0).fit(X, y)

print("root Gini :", round(clf.tree_.impurity[0], 4))   # 0.4592
print("root split:", X.columns[clf.tree_.feature[0]])    # age
print(export_text(clf, feature_names=list(X.columns)))
# The printed tree starts by splitting on 'age' — binary (age <= 0.5, etc.),
# but the same logic as the drawn 3-way tree in content/01.
```

**Expected:** `root Gini : 0.4592`, `root split: age`. If you see those two lines, the hand-maths and the library agree.

---

## Step 2 — Watch a single tree overfit (~5 min)

```python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split

data = load_breast_cancer()
Xtr, Xte, ytr, yte = train_test_split(
    data.data, data.target, test_size=0.25, random_state=42)

tree = DecisionTreeClassifier(random_state=42).fit(Xtr, ytr)   # unconstrained
print("train:", round(tree.score(Xtr, ytr), 3))   # 1.0    <- memorised
print("test :", round(tree.score(Xte, yte), 3))   # ~0.909 <- the overfitting gap
print("depth:", tree.get_depth())                 # ~7     <- grew deep to do it
```

**Expected:** train 1.000, test ~0.91. The gap *is* overfitting — perfect on seen data, worse on new.

---

## Step 3 — Fix it with a random forest + OOB (~6 min)

```python
from sklearn.ensemble import RandomForestClassifier

forest = RandomForestClassifier(
    n_estimators=300, max_features="sqrt",
    oob_score=True, random_state=42, n_jobs=-1).fit(Xtr, ytr)

print("test    :", round(forest.score(Xte, yte), 3))   # ~0.965  up from 0.909
print("oob     :", round(forest.oob_score_, 3))        # ~0.958  free validation
```

**Expected:** test ~0.97 (up from ~0.91), OOB ~0.96 close to test. The forest averaged the variance away, and the OOB score predicted the improvement *without* using the test set.

---

## Step 4 — Read what survived: feature importances (~4 min)

```python
import numpy as np
from sklearn.inspection import permutation_importance

# Built-in (fast, but biased toward high-cardinality features)
for i in np.argsort(forest.feature_importances_)[::-1][:5]:
    print(f"{data.feature_names[i]:<25} {forest.feature_importances_[i]:.3f}")

print("---")
# Permutation importance (slower, more trustworthy — measured on the test set)
perm = permutation_importance(forest, Xte, yte, n_repeats=10, random_state=42)
for i in np.argsort(perm.importances_mean)[::-1][:5]:
    print(f"{data.feature_names[i]:<25} {perm.importances_mean[i]:.3f}")
```

**Expected:** both lists put "worst"-prefixed size features (perimeter, concave points, area) on top. When the two rankings roughly agree, that's reassuring; when they disagree, trust the permutation version.

---

## Step 5 — Draw a tree you can actually read (~4 min)

```python
import matplotlib.pyplot as plt
from sklearn.tree import plot_tree

readable = DecisionTreeClassifier(max_depth=3, random_state=42).fit(Xtr, ytr)
print("test:", round(readable.score(Xte, yte), 3))   # ~0.930 — less accurate, fully readable

plt.figure(figsize=(16, 8))
plot_tree(readable, feature_names=data.feature_names,
          class_names=list(data.target_names), filled=True, rounded=True, fontsize=8)
plt.show()
```

**Expected:** a flowchart where each box shows its split rule, its `gini`, sample count, and class split — machine-drawn, same anatomy as `content/01`. This depth-3 tree scores ~0.93: a couple of points below the forest, but you can read every decision.

---

## Now break it / now extend it

1. **Break the forest back into overfitting.** Set `max_features=None` (each split sees *all* features) and re-run Step 3. Test accuracy typically drops a little — the trees become more similar (correlated), so voting helps less. This is *why* feature randomness matters.
2. **Shrink the forest and watch OOB get noisy.** Set `n_estimators=10`. Compare `oob_score_` to the test score across a few `random_state` values — with few trees, OOB wobbles, because some rows have too few out-of-bag trees to vote. Confirms the caveat from `content/03`.
3. **Find the interpretability breaking point.** Sweep `max_depth` from 1 to 10 on the single tree, plotting train and test accuracy. Where does test accuracy peak, and is that tree still small enough to read on one page? That gap between "most accurate depth" and "still readable depth" is the trade this session is about.
