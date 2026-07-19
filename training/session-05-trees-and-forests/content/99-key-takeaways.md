# Key Takeaways

A tight recap of Session 5. If you read nothing else, read this.

## The mechanism, in five sentences

1. A **decision tree** is a flowchart of yes/no questions the machine *learns* from labelled data; you predict by walking root-to-leaf, and you can read the reason at every fork.
2. The tree chooses each question by minimising **Gini impurity** — a "how mixed is this group" cost that is the same **cost/distance** idea as MSE in regression and log-loss in classification, applied to class probabilities.
3. Gini impurity is `1 − Σ pᵢ²`: **0 = pure**, **0.5 = a 50/50 two-class split (maximally mixed)**; a split is scored by the *weighted* impurity of its children, and the lowest wins.
4. A single tree left to grow **overfits** — it memorises the training data (100% train accuracy, worse on new data) because it is a low-bias, **high-variance** model.
5. A **random forest** grows many deliberately-different trees (each on a **bootstrap** sample, each split seeing a random feature subset) and takes a **majority vote** — averaging the variance away, with **out-of-bag (OOB)** error as a free validation estimate.

## The numbers worth remembering

| Fact | Value | Why it matters |
|---|---|---|
| Gini of a pure node | **0** | the target of every split |
| Gini of a 50/50 two-class node | **0.5** | the most costly / mixed case |
| Root Gini of the buys-computer set | **0.4592** | the number you re-derived by hand and in code |
| Rows left out of each bootstrap sample | **~37%** (→ 1/e) | these are the OOB set — free validation |
| `feature_importances_` caveat | biased to high-cardinality | prefer `permutation_importance` for real claims |

## Tree vs. forest — pick with eyes open

| | Single tree | Random forest |
|---|---|---|
| Accuracy / stability | lower, unstable | higher, steady |
| **Read one decision?** | **yes — full path** | no — importances only |
| Validation | needs a holdout | OOB for free |
| Use when | the decision must be **defensible** | you need accuracy behind a human gate |

## Why this session exists (the role point)

Trees and forests are the **interpretable** family — a deliberate contrast to the black-box LLM. For release, problem, and configuration management, a tree hands you a **complete, checkable justification** for every prediction: *"predicted yes because senior + fair credit, and every such case in our data bought."* An LLM can only give you a fluent story it generated, which may not be the real reason. A random forest buys accuracy back by voting many trees — but pays for it with the very readability that made one tree special, keeping only a model-level feature ranking. Knowing when that trade is worth making is the skill.

## If you remember one thing

> **A decision tree shows its work — and an explanation you can check beats an answer you have to trust.** Reach for a tree first when the decision has to be defensible and the data is tabular; move to a forest for accuracy when you can live with a human review gate instead of a readable path.
