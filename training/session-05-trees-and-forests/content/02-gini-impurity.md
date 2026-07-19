# Gini Impurity — How the Tree Picks Each Split

The previous file left one question open: when the tree considers every possible question, how does it score them? It needs a number for *how mixed* a group of examples is. That number is **Gini impurity**, and the good news for this course is that it is not a new idea — it is the same **cost / distance** concept you have already met, wearing a different costume.

## The one recurring idea

Throughout this series, learning is "reduce a cost":

| Method | The "cost" it minimises | What the cost measures |
|---|---|---|
| Regression | Mean squared error (MSE) | how far predictions are from true numbers |
| Classification (logistic) | Log-loss | how far predicted probabilities are from true labels |
| Reinforcement learning | Bellman error | how far value estimates are from reality |
| Neural networks | a loss, pushed backward | how wrong the whole network is |
| **Decision trees** | **Gini impurity** | **how mixed the class labels in a group are** |

The framing *"the same concept of 'distance' or 'cost' we saw before, applied to probabilities"* is worth stating out loud in the room. A tree is a cost-minimiser like everything else; its cost just happens to be "how mixed is this pile of examples."

## What Gini impurity is

Take a group of examples. Look at the fraction of each class in it — call them p₁, p₂, … Gini impurity is:

```
Gini = 1 − Σ (pᵢ)²
```

That is: **one minus the sum of the squared class fractions.** For a two-class problem (buys / doesn't buy) with fractions p and (1 − p):

```
Gini = 1 − p² − (1 − p)²
```

### An intuition you can say in one breath

Gini impurity is the probability you would **mislabel** a randomly-picked example if you labelled it by drawing a class at random from the group's own mix. A pure group (all one class) → you can't get it wrong → Gini 0. A perfectly mixed group (50/50) → you're wrong half the time → Gini 0.5. That is why "impurity" and "cost" are the same word here: a mixed group is a costly group, because it cannot yet commit to an answer.

| Group makeup (2 classes) | p | Gini = 1 − p² − (1−p)² | Read as |
|---|---|---|---|
| all "yes" | 1.0 | **0.0** | pure — no cost |
| 9 yes / 1 no | 0.9 | 0.18 | nearly pure |
| 3 yes / 1 no | 0.75 | 0.375 | leaning yes |
| 2 yes / 3 no | 0.4 | 0.48 | messy |
| 50 / 50 | 0.5 | **0.5** | maximally mixed — most costly |

For two classes Gini runs from 0 (pure) to 0.5 (even split). With *k* classes the worst case is 1 − 1/k.

> **Correcting a source-deck slip.** The excluded Cisco deck's Gini section had a transcription error printing **0.5 for a pure split** (`AI_input.md` §6, defect #11). A pure split has Gini **0**, not 0.5 — 0.5 is the *maximally impure* two-class case. If you have seen that slide, unlearn it.

## Scoring a split: weighted impurity

Impurity scores one group. A *split* creates several child groups, so we score a split by the **weighted average impurity of its children** — each child weighted by how many examples fell into it:

```
Gini(split) = Σ over children  (n_child / n_total) × Gini(child)
```

The tree picks the split with the **lowest** weighted child impurity (equivalently, the largest *drop* from the parent's impurity — the "Gini gain"). Lowest mixing wins. That is the entire decision rule.

## Worked example: choosing the root split

Back to the 14-row "buys a computer?" table (`01`). Nine bought, five did not, so the impurity of the whole set before any split is:

```
Gini(root) = 1 − (9/14)² − (5/14)²
           = 1 − 0.4133 − 0.1276
           = 0.4592
```

Now score each candidate feature as the first question. Take **age** in full, because it wins.

**Split on `age`** — three children:

| age value | rows | yes / no | Gini of child |
|---|---|---|---|
| youth | 1,2,8,9,11 | 2 / 3 | 1 − (2/5)² − (3/5)² = **0.480** |
| middle_aged | 3,7,12,13 | 4 / 0 | 1 − 1² − 0² = **0.000** (pure!) |
| senior | 4,5,6,10,14 | 3 / 2 | 1 − (3/5)² − (2/5)² = **0.480** |

Weighted impurity of the age split:

```
(5/14)(0.480) + (4/14)(0.000) + (5/14)(0.480)
= 0.1714 + 0 + 0.1714
= 0.3429
```

Now the same arithmetic for the other three features gives the full scoreboard:

| Candidate first question | Weighted Gini after split | Gini gain (0.4592 − this) | Rank |
|---|---|---|---|
| **age** | **0.3429** | **0.1163** | **1 — chosen** |
| student | 0.3673 | 0.0919 | 2 |
| credit_rating | 0.4286 | 0.0306 | 3 |
| income | 0.4405 | 0.0187 | 4 |

**age wins** — it produces the least-mixed children (and note it already hands us one *pure* child: every middle-aged customer bought, so that branch is done immediately). This is exactly why the tree in file `01` starts with age. The machine did not know age "should" matter; it tried all four and this one cut the cost the most.

### Recursing: the youth branch

The tree now repeats the same scoring *within* each impure child. Take the **youth** group (rows 1, 2, 8, 9, 11 — 2 yes, 3 no, Gini 0.480). Score the remaining features on just these five rows. Splitting on **student** gives:

| student | youth rows | yes / no | Gini |
|---|---|---|---|
| no | 1, 2, 8 | 0 / 3 | **0.000** (pure) |
| yes | 9, 11 | 2 / 0 | **0.000** (pure) |

Weighted impurity = 0. A perfect split — both children pure — so *student* is chosen for the youth branch and both sides become leaves. The senior branch works out identically with *credit_rating* (fair → all yes, excellent → all no). That reproduces the whole tree from `01`, every fork justified by a number.

## From the hand-computation to scikit-learn

scikit-learn's `DecisionTreeClassifier` uses exactly this — Gini is its **default** split criterion (`criterion="gini"`; the alternative, `criterion="entropy"`, uses information gain and usually picks nearly identical splits). You will see the impurity at every node printed right on the tree:

```python
from sklearn.tree import DecisionTreeClassifier, export_text
import pandas as pd

# The 14-row table from file 01, encoded as integers
# (scikit-learn needs numbers; the mapping is in exercises/lab.md)
clf = DecisionTreeClassifier(criterion="gini", random_state=0)
clf.fit(X, y)          # X: the four features, y: buys_computer
print(export_text(clf, feature_names=list(X.columns)))

# Expected (abridged) — every node reports the split it chose:
# |--- age <= 0.5           <- the root split is on age, exactly as computed
# |   |--- ... 
# The classifier's root impurity, clf.tree_.impurity[0], is ~0.459
# — the 0.4592 we computed by hand.
```

The number your hand produced (0.4592 at the root) is the number the library reports. The mechanism is not hidden; you just re-derived it.

## Two honest caveats

- **Greedy, per-node.** Gini scores each split *locally*. As noted in `01`, a chain of locally-best splits is not guaranteed to be the globally-best tree — only a good, fast one.
- **Gini vs. entropy rarely matters.** People argue about Gini versus information-gain (entropy); in practice they choose the same or near-identical splits, and Gini is slightly cheaper to compute (no logarithm). Do not let a vendor make this sound like a deep distinction.
