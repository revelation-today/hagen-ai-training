# Session 5 — Methods III: Decision Trees & Random Forests

**Series:** AI Training for Qualcomm (Release / Problem / Configuration Management + Developers)
**Block:** Know the Methods (Sessions 3–6) · **Goal 3** (methods explained)
**Format:** 45 min content + 15 min Q&A · English · Python (scikit-learn)

---

## One-paragraph summary

This session teaches the **most interpretable** family of machine-learning models — a deliberate contrast to the black-box LLM the rest of the series circles around. A **decision tree** is a flowchart the machine *learns* from data: a chain of yes/no questions ending in an answer, and you can read every step. We show how a tree decides which question to ask first using **Gini impurity** — the same "cost / distance" idea that runs like a spine through every method in this course, here applied to class probabilities. Then we show the catch: a single tree left to grow freely *memorises* its training data and fails on new data. The fix — a **random forest** — grows many deliberately-different trees on resampled data and lets them vote. The payoff for this audience: a tree *shows its reasoning*. An auditable model you can read and defend in a change-review is often worth more than a more accurate one you cannot.

## Audience & level

Qualcomm release / problem / configuration managers and developers with some prior AI exposure. Technically literate; not everyone codes daily. The Python is readable and every line is explained; the statistics are worked by hand before any library is called. **Role hook:** trees and forests are the models you would actually reach for on *tabular* operational data — incident attributes, config parameters, release metrics — and the only common family whose decisions you can audit line by line.

## Learning objectives

By the end of this session a participant can:

1. **Explain** a decision tree as a learned flowchart, and read one to justify a single prediction.
2. **Compute** Gini impurity for a node by hand and use it to say *why* a tree picks one split over another.
3. **Connect** Gini impurity to the "cost / distance" idea seen in regression and classification — one recurring concept, not a new one.
4. **Explain** why a single unconstrained tree overfits, and how bootstrap + bagging + feature randomness in a random forest fix it.
5. **Interpret** an out-of-bag (OOB) score and a feature-importance ranking, and state the honest caveats of each.
6. **Decide** when an auditable tree/forest is the right choice over a more accurate but opaque model — in release/problem/config terms.

## Prerequisites

- **Session 3 — Methods I: Learning From Data** (supervised learning, features/labels, the train/validation/test split, regression vs. classification). This session assumes those terms.
- Helpful but not required: **Session 4 — Unsupervised Learning** (same scikit-learn workflow, same "cost" spine).
- No calculus. Arithmetic and reading a small table of numbers is enough.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook** — the model you can read | Contrast: an LLM cannot tell you *why*; a tree can. |
| 3–8 min | **A tree is a learned flowchart** | The "buys a computer?" worked example; read one path. |
| 8–18 min | **How the tree picks a split** | Gini impurity by hand; the root-split table; the cost/distance spine. |
| 18–25 min | **Why one tree overfits** | A tree grown to purity memorises; live demo of train 100% / test lower. |
| 25–37 min | **From one tree to a forest** | Bootstrap → bagging → feature randomness → OOB → majority vote. |
| 37–43 min | **Why this matters to your role** | Interpretability made concrete; tree/forest vs. neural net/LLM. |
| 43–45 min | **Recap** | Key takeaways + the one thing to remember. |
| 45–60 min | **Q&A** | See `exercises/discussion.md`. |

Honest timing note: the Gini segment (8–18) is the tightest. If the room is not comfortable with the arithmetic, cut the second and third worked splits and keep only the root-split table — the mechanism lands from one example.

## Materials & tools

- **Self-study reading:** `content/00-overview.md` → `content/99-key-takeaways.md`, in order.
- **Live demo / lab:** `exercises/lab.md` — a ~20–30 min scikit-learn notebook (Colab-first; JupyterLite fallback). Builds the "buys a computer?" tree, then a random forest with `plot_tree`, `feature_importances_`, and `oob_score_`.
- **Deck spec:** `slides/outline.md` (built per `../../powerpoint_instructions.md`).
- **Link-only live demo:** r2d3.us "A Visual Introduction to Machine Learning" — run it, do not screenshot (see licence note).

## Source & licence note

This topic existed in the corpus **only** in the excluded `Cisco Confidential` deck (see `../../AI_input.md` §1 and the proposal §2), so the whole session is **rebuilt from clean public sources**. The build-from source is **scikit-learn's user guide and examples gallery — BSD-3-Clause, confirmed slide-safe** for both prose and generated figures. The classic "buys a computer?" dataset is a standard textbook example (Han, Kamber & Pei, *Data Mining*) reproduced here as a small original table. **Link-only** (reference / live-demo, never copied onto a slide): StatQuest videos and r2d3.us. Full verdicts in `resources/sources.md`.
