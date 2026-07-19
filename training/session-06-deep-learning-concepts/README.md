# Session 6 — Methods IV: Deep Learning, Conceptually

**Block:** Methods · **Goal:** 3 (know the methods) · **Format:** 45 min content + 15 min Q&A · **Hands-on:** none (Session 7 is the lab)

---

## Summary

This is the session that demystifies the neural network. By the end, the audience should be able to say, honestly, *"I know what is happening inside one of these."* We build the whole picture from one idea — a **neuron is a weighted sum followed by a nonlinearity** — stack those into **layers**, and watch numbers flow through a tiny concrete network that predicts whether a background colour needs **light or dark text**. Then we explain **training**: how a network starts out useless (random weights, chance accuracy) and gets good, by nudging its weights to reduce error. Crucially, we teach gradient descent and backpropagation **by intuition, not by calculus** — the flashlight-in-the-mountains metaphor does the work the derivatives would. This session deliberately shows the machinery without deriving it, so that Sessions 7–8 (hands-on Keras) can be practical without hand-waving.

## Audience & level

Qualcomm release / problem / configuration managers and developers, with the AI/ML grounding from Sessions 1–5. Technical but not all coders. No calculus is required or used. The one small code block (Keras) is illustrative — nobody has to run anything this session.

## Learning objectives

By the end, a participant can:

- **Explain** what a single neuron computes: a weighted sum of its inputs, plus a bias, passed through an activation function.
- **Describe** how neurons compose into layers, and state precisely what "deep" means (more than one hidden layer) — including why our own example is technically *not* deep.
- **Trace** a forward pass through the 3→3→1 colour network by hand, following RGB in to one probability out.
- **Justify** why activation functions (nonlinearity) are essential — what breaks without them.
- **Explain** training as error-reduction: loss, gradient descent, and backpropagation, at the intuition level, using the flashlight metaphor — no derivatives.
- **Explain** overfitting and why we hold out a test set.

## Prerequisites

- **Session 1** (what AI is; hype vs. reality) and **Session 3** (supervised learning; the idea of a *loss/cost* to minimise, and the *train/test split*). This session leans directly on both.
- **Sessions 4–5** are helpful context: the "cost/distance we minimise" spine runs through every method, and it returns here as *loss*.
- No calculus, no linear algebra beyond "a grid of numbers." Python literacy helps for the one snippet but is not required.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–4 min | **Hook** | The colour-picker problem: given any background, pick readable text. A rule works — but let's watch a network learn it. |
| 4–11 min | **One neuron** | Weighted sum + bias + activation. A neuron is "a linear function wearing a nonlinearity." |
| 11–18 min | **A network of layers** | 3→3→1; input/hidden/output; what "deep" means; our example is *not* deep — and that's fine. |
| 18–26 min | **Forward propagation** | Push one real colour through, by hand: RGB → hidden → output → one probability. The ≥.5 → DARK rule. |
| 26–32 min | **Activation functions** | Why nonlinearity matters at all (remove it and the network collapses to a line). ReLU / sigmoid / tanh / softmax comparison. |
| 32–41 min | **Training** | Random start = chance accuracy. Loss. Gradient descent = flashlight in the mountains. Backprop = distribute the blame backward. Shown, not derived. |
| 41–45 min | **Overfitting & test data** | Memorising ≠ learning; hold out a test set; recap. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. |

The 45 minutes is comfortable if the forward-pass worked example is pre-drawn rather than computed live. If time is tight, compress activation functions to the one-line "without nonlinearity, layers collapse" point and keep the training segment whole — it is the heart of the session.

## Materials & tools

- Slides: `slides/outline.md` (built per `../powerpoint_instructions.md`).
- Self-study reading: `content/00-overview.md` → `content/99-key-takeaways.md`.
- Reflective exercise (no lab): `exercises/lab.md`. Self-check: `exercises/quiz.md`.
- **Pre-reading (assign, do not embed):** 3Blue1Brown's *Neural Networks* series, chapters 1–4 — the best visual intuition for exactly this material. Link-only; see `resources/sources.md`.
- Optional live demo: the Desmos activation-function graphs (a tool, shown live, not copied onto a slide).

## Source & licence note

This session re-pitches the **conceptual layer** of Thomas Nield's *Deep Learning for Beginners* Days 1–2 (O'Reilly) from "build it from scratch in NumPy" to "understand it." The source decks are **LINK-ONLY** (commercial, all-rights-reserved) — every concept here is re-authored in our own words and figures; nothing is reproduced. The one Keras snippet follows the standard TensorFlow/Keras API (Apache-2.0, **SLIDE-SAFE** as a pattern). 3Blue1Brown is **LINK-ONLY** (assign as pre-reading). Full verdicts and the corrected source errors (notably the ≥.5 light/dark threshold contradiction — we use **≥.5 → DARK**) are in `resources/sources.md`.
