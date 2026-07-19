# Session 3 — Methods I: Learning From Data

**Block:** Methods (Sessions 3–6) · **Goal covered:** 3 (Methods explained — foundation) · **Format:** 45 min content + 15 min Q&A

---

## Summary

This is the foundation session for the "methods" block. Before Sessions 4, 5, and 6 cover specific method families (unsupervised learning, decision trees/random forests, deep learning), this session teaches the machinery they all share, so those sessions don't each re-explain it. You will learn what **supervised learning** actually is (learning rules from examples, not being programmed with rules), what **a model** is, why we deliberately **hold data back** before we trust a model, the difference between **regression and classification** and how a probability turns into a yes/no decision, and — the takeaway this audience will use most — a **decision heuristic for which kind of model fits which kind of problem**. It is a concept session: no full lab, but a short scikit-learn illustration shows train-versus-test accuracy in about fifteen lines.

For release, problem, and configuration managers, the payoff is a working vocabulary and a set of hard questions to ask any team or vendor who says "we trained a model": *What was it trained on? What data did you hold back? What does it output, and where's the threshold? Is this even a problem that needs a neural network?*

## Audience & level

Qualcomm release / problem / configuration managers and developers, with some prior AI exposure. Technical but not all coders. Sessions 1–2 are assumed (the AI ⊃ ML ⊃ DL ⊃ LLM vocabulary, tokens, training vs. inference named). No calculus, no prior ML method knowledge required. The one short code illustration is readable without Python fluency.

## Learning objectives

By the end of this session a participant can:

1. **Explain** the inversion at the heart of machine learning — traditional software is *rules + data → answers*; supervised learning is *data + answers → rules* — and define **features** and **labels** with a worked example.
2. **Describe** what "a model" is using the dark-clouds-mean-rain intuition, and distinguish **training cost** (one-time, expensive) from **inference cost** (per-prediction, cheap).
3. **Justify** the train / validation / test split (70 / 15 / 15) — *why* you hold data back — and explain what goes wrong when you don't (overfitting, a metric that lies).
4. **Distinguish** regression from classification, and trace **how a probability becomes a decision** through a threshold, including why the threshold is a business choice, not a technical constant.
5. **Apply** the decision heuristic — *structured/tabular data → simple models; perceptual/fuzzy problems → neural networks; use the simplest model that works* — to a handful of real Qualcomm-flavoured problems.

## Prerequisites

- **Session 1** — "learning by example, not by rules"; the reconstruction/pattern-matching mental model.
- **Session 2** — the AI ⊃ ML ⊃ DL ⊃ LLM nesting; tokens; the terms *model*, *training*, *inference*, *parameters vs. hyperparameters*.
- No maths beyond arithmetic. No coding required to follow, though the illustration is Python.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | Content |
|---|---|---|
| 0–4 min | Hook | "Data + answers → rules." The inversion. One slide that reframes everything in the Methods block. |
| 4–12 min | Supervised learning | Features, labels; the supervised-learning loop; a worked tabular example. |
| 12–19 min | What a model is | Dark-clouds-mean-rain; parameters as learned dials; training cost vs. inference cost. |
| 19–29 min | Holding data back | Overfitting = memorising the exam; the 70/15/15 split; what validation is for; the scikit-learn train-vs-test demo. |
| 29–37 min | Regression vs. classification | Continuous vs. label; how a probability crosses a threshold to become a decision; the threshold is a business lever. |
| 37–45 min | Which model for which problem | The decision heuristic; structured vs. perceptual; "simplest model that works"; live decision poll. |
| 45–60 min | Q&A | Discussion prompts in `exercises/discussion.md`. |

**Honest timing note:** this is a full 45 minutes with no slack. If the room is new to the material, the split section (19–29) is the one to protect and the regression/classification worked numbers are the first thing to trim. The demo is ~2 minutes if pre-run; do not live-type it.

## Materials & tools

- Slides: `slides/outline.md` (built per `../powerpoint_instructions.md`).
- Reading: `content/00-overview.md` → `content/05-…` → `content/99-key-takeaways.md`.
- Reflective exercise + optional 15-line scikit-learn illustration: `exercises/lab.md` (this is a concept session — no full lab).
- Self-check: `exercises/quiz.md`. Discussion/poll prompts: `exercises/discussion.md`.
- Demo (optional, pre-run): scikit-learn `train_test_split` + a decision-tree stump on a toy dataset, showing train accuracy > test accuracy. Colab-first; JupyterLite fallback.

## Source & licence note

The conceptual framing (the *data + answers → rules* inversion, the when-to-use-neural-networks heuristic, regression-vs-classification, "use the simplest model that works") is drawn from **Deep Learning for Beginners — Day 1, §I** (Thomas Nield, O'Reilly). That deck is **all-rights-reserved → LINK-ONLY**: assign it as reading or reference the ideas in our own words, but do not reproduce its slides. All code and any figures on slides use **scikit-learn (BSD-3-Clause) → SLIDE-SAFE**. The 70/15/15 three-way split is standard textbook practice, presented in our own words (the source deck used a two-way 2/3–1/3 split; we add the validation third and say why). One source-deck error is corrected in-session: the light/dark threshold contradiction (see `content/04-regression-vs-classification.md`). Full verdicts in `resources/sources.md`.
