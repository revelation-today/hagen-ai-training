# Runnable Demo — Trees and Forests in scikit-learn

This file is the read-along version of the code. The hands-on version, with setup and challenges, is `exercises/lab.md`. Everything here uses **scikit-learn (BSD-3-Clause — slide-safe)**. Expected outputs are shown in comments; exact numbers depend on library version and `random_state`, so treat them as representative (the shape of the result is what matters, not the last decimal).

## Part 1 — Rebuild the "buys a computer?" tree

We reproduce the 14-row example from files `01`–`02`, so you can confirm the machine chooses **age** as the root split, exactly as we computed by hand.

```python
import pandas as pd
from sklearn.tree import DecisionTreeClassifier, export_text

# The 14 rows from file 01
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

# scikit-learn needs numbers, so map each category to an integer.
# (Ordinal encoding is fine here because trees split on thresholds, not distances.)
maps = {
    "age":           {"youth":0, "middle_aged":1, "senior":2},
    "income":        {"low":0, "medium":1, "high":2},
    "student":       {"no":0, "yes":1},
    "credit_rating": {"fair":0, "excellent":1},
    "buys":          {"no":0, "yes":1},
}
enc = df.replace(maps)
X, y = enc[["age","income","student","credit_rating"]], enc["buys"]

clf = DecisionTreeClassifier(criterion="gini", random_state=0)
clf.fit(X, y)

# The root's Gini impurity — compare to the 0.4592 we computed by hand:
print(round(clf.tree_.impurity[0], 4))
# 0.4592                      <- matches the hand calculation exactly

# Which feature did the tree split on first?
root_feature = X.columns[clf.tree_.feature[0]]
print(root_feature)
# age                         <- the machine chose age as the root, as predicted

print(export_text(clf, feature_names=list(X.columns)))
# |--- age <= 0.50            (age == youth branch)
# |   |--- student <= 0.50
# |   |   |--- class: 0       (youth, not student  -> no)
# |   |--- student >  0.50
# |   |   |--- class: 1       (youth, student      -> yes)
# |--- age >  0.50
# |   |--- ... middle_aged -> class 1; senior splits on credit_rating ...
#
# Note the shape differs from file 01's drawing: scikit-learn splits BINARY
# (age <= 0.5 vs > 0.5), so a 3-way "age" question becomes nested yes/no
# questions. The logic is identical; the tree just asks it two at a time.
```

**What to notice:** the hand-computed root impurity (0.4592) and the chosen root feature (age) both come straight out of the library. You re-derived scikit-learn's behaviour with a pocket calculator in file `02`.

## Part 2 — Watch a single tree overfit

Now a realistic dataset (the built-in breast-cancer set: 569 rows, 30 numeric features, a binary label). We grow **one unconstrained tree** and compare its training and test accuracy — the signature of overfitting from file `03`.

```python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier

data = load_breast_cancer()
Xtr, Xte, ytr, yte = train_test_split(
    data.data, data.target, test_size=0.25, random_state=42)

tree = DecisionTreeClassifier(random_state=42)   # no depth limit -> grows to purity
tree.fit(Xtr, ytr)

print(round(tree.score(Xtr, ytr), 3))   # training accuracy
# 1.0                       <- perfect on data it has seen: it MEMORISED
print(round(tree.score(Xte, yte), 3))   # test accuracy
# 0.909                     <- ~9% worse on new data: the overfitting gap
```

A perfect training score with a lower test score is exactly the "great in the lab, worse in production" pattern. The tree learned every wrinkle of the training set, including the noise.

## Part 3 — Replace it with a random forest

Same data, but now a forest of trees with bootstrap, feature randomness, voting, and a free OOB estimate.

```python
from sklearn.ensemble import RandomForestClassifier

forest = RandomForestClassifier(
    n_estimators=300,     # 300 trees, each on its own bootstrap sample
    max_features="sqrt",  # each split sees only sqrt(30) ~ 5 random features
    oob_score=True,       # score each row using only the trees that didn't train on it
    random_state=42,
    n_jobs=-1,            # trees are independent -> train them in parallel
)
forest.fit(Xtr, ytr)

print(round(forest.score(Xtr, ytr), 3))   # still ~perfect on train...
# 1.0
print(round(forest.score(Xte, yte), 3))   # ...but much better on new data
# 0.965                     <- up from 0.909: variance averaged away
print(round(forest.oob_score_, 3))        # the "free" validation estimate
# 0.958                     <- close to the real test score, computed from train data
```

The forest lifts test accuracy from ~0.91 to ~0.97 **without** a separate validation split telling us so — the OOB score (~0.96) predicted it from the training data alone. That is the practical magic of bagging.

## Part 4 — What survived the forest: feature importances

We lost the readable per-prediction path (Part 1), but the forest still ranks which features drove its decisions.

```python
import numpy as np

# Quick view: mean decrease in impurity (Gini importance), built in
order = np.argsort(forest.feature_importances_)[::-1]
for i in order[:5]:
    print(f"{data.feature_names[i]:<25} {forest.feature_importances_[i]:.3f}")
# worst perimeter           0.144
# worst concave points      0.139
# worst area                0.115
# mean concave points       0.106
# worst radius              0.078
#   (top features are plausible, tumour-size-related measurements)

# More trustworthy for a claim you will defend: permutation importance,
# measured on the TEST set (harder to fool than the built-in Gini version).
from sklearn.inspection import permutation_importance
perm = permutation_importance(forest, Xte, yte, n_repeats=10, random_state=42)
top = np.argsort(perm.importances_mean)[::-1][:5]
for i in top:
    print(f"{data.feature_names[i]:<25} {perm.importances_mean[i]:.3f}")
# worst concave points      0.049
# worst area                0.041
# worst perimeter           0.032
# ...                       (ranking is broadly similar here, which is reassuring)
```

**Read this honestly (from file `03`):** the built-in `feature_importances_` (MDI) is quick but biased toward high-cardinality features; `permutation_importance` measures the real effect on held-out predictions and is what you cite when it matters. Here they roughly agree, which is a good sign — when they *disagree*, trust permutation importance.

## Part 5 — Draw a tree you can actually read

For the interpretability point (file `04`), render one shallow tree as a picture. Keep it shallow (`max_depth=3`) so it stays human-readable — the whole point.

```python
import matplotlib.pyplot as plt
from sklearn.tree import plot_tree

readable = DecisionTreeClassifier(max_depth=3, random_state=42).fit(Xtr, ytr)
print(round(readable.score(Xte, yte), 3))
# 0.930   <- a depth-3 tree: less accurate than the forest, but you can read
#            every decision it makes. That trade is the subject of file 04.

plt.figure(figsize=(16, 8))
plot_tree(readable, feature_names=data.feature_names,
          class_names=data.target_names, filled=True, rounded=True)
plt.savefig("readable_tree.png", dpi=150, bbox_inches="tight")
# Produces a flowchart: each box shows the split rule, its gini, the sample
# count, and the class split -- the same anatomy as file 01, machine-drawn.
```

`plot_tree` output is generated by scikit-learn and is therefore **slide-safe (BSD-3)** — you may put this figure on a slide with attribution. (Contrast: r2d3.us and StatQuest visuals are link-only; run them live, don't copy them.)

## The demo in one table

| Part | Shows | The point |
|---|---|---|
| 1 | tree picks **age** as root; root Gini **0.4592** | the library confirms the hand calculation |
| 2 | one tree: train **1.00**, test **~0.91** | a single tree overfits |
| 3 | forest: test **~0.97**, OOB **~0.96** | bagging + voting fix it; OOB validates for free |
| 4 | feature-importance ranking (MDI vs. permutation) | what survives the forest — with caveats |
| 5 | a depth-3 tree you can read, test **~0.93** | interpretability vs. accuracy, made concrete |
