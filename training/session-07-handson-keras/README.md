# Session 7 — Hands-On I: Build and Train a Network in Keras

**Block:** Do · **Goal 4 (first half)** · **Format:** hands-on lab (the lab *is* the session) · 45 min + 15 min Q&A

---

## One-paragraph summary

Session 6 explained what a neural network is. This session makes one, and it makes it **yours**. In a Google Colab notebook, every person in the room loads a real dataset of 1,300-odd background colours labelled "needs light text" / "needs dark text", builds a `3 → 3 → 1` network in about five lines of `tf.keras`, compiles it, calls `fit()`, and watches accuracy climb from chance to the mid-90s in under a minute of compute. Every single line is explained — `Dense`, `relu`, `sigmoid`, `epochs`, `batch_size`, the `/255` scaling, the loss function — and **nothing is derived**: no calculus, no NumPy-from-scratch, no matrix algebra. The centrepiece is deliberately staged: we **evaluate the model before training it**, so the room sees a randomly-initialised network score at chance and predict the same answer for every colour, and only then do we train. Learning is not described; it is *witnessed*. The session ends with a working notebook that Session 8 picks up unchanged.

## Audience & level

Qualcomm release / problem / configuration managers and developers who have sat through Sessions 3 and 6. **You do not need to be a Python programmer.** Every cell is provided in full; the rhythm is type-along, and the "typing" is mostly changing one number and pressing Shift+Enter. **Developers** will find the code trivially easy and should spend their spare attention on the challenges at the end of the lab. **Managers:** the value for you is not the syntax — it is seeing, concretely, how small the thing is. A production ML system's core is fifteen lines. Everything else in this course (data quality, validation, metrics, deployment risk) is the part that is actually hard, and knowing how short *this* part is recalibrates a lot of vendor conversations.

## Learning objectives

By the end, a participant can:

1. **Build** a `Sequential` `3 → 3 → 1` binary classifier in `tf.keras` and explain what every argument does.
2. **Explain** why input features are scaled (`/255`) and what happens to training when they are not.
3. **Distinguish** the four stages — build → compile → fit → evaluate — and say what each one is responsible for.
4. **Demonstrate** that an untrained network performs at chance, and interpret *why* it predicts one class for everything.
5. **Read** a Keras training log: epoch, loss, accuracy, and what "it's still improving" versus "it's flat" looks like.
6. **Change** one hyperparameter at a time (`epochs`, `batch_size`, hidden units, learning rate) and describe the effect on the result.
7. **Make a prediction** on a colour of their own choosing and apply the decision rule **≥ 0.5 → DARK text**.

## Prerequisites

- **Session 6** — what a neuron, a layer, an activation function, a loss, and gradient descent are, conceptually. This session assumes all of it and derives none of it.
- **Session 3** — supervised learning, labels and features, the train/test split and why data is held back.
- **A Google account** for Colab. Nothing to install; no GPU needed (these models are tiny — CPU trains them in seconds).
- Python literacy is helpful but **not required**. Every cell is given complete.

## Agenda (45 min delivery + 15 min Q&A)

This is a lab. The presenter drives a Colab notebook on screen; the room runs the same cells. Each segment is *run it, then debrief it*. The rhythm is the one that works: **hold one code block on screen, change one thing, re-run, discuss what moved.**

| Time | Segment | What happens |
|---|---|---|
| 0–4 min | **Hook & setup** | Everyone into Colab and running Cell 0. The promise: nobody leaves this room without having trained a model. |
| 4–9 min | **The data** | Load the colour CSV, look at it, scale by 255, check which label means "dark". |
| 9–16 min | **Build it — five lines** | `Sequential`, two `Dense` layers, `relu`, `sigmoid`. `model.summary()` → **16 parameters**. That's the whole model. |
| 16–21 min | **Compile it** | Loss, optimizer, metric — what each of the three is actually for. |
| 21–27 min | **The honest moment** | `evaluate()` **before** `fit()`. Chance accuracy. Then the reveal: it predicted the *same class for every colour*. |
| 27–36 min | **`fit()` — watch it learn** | Run 100 epochs. Read the log live. Then change one thing (`epochs=5`) and re-run to feel the difference. |
| 36–42 min | **Evaluate & predict** | Score on the held-out test set. Predict a colour the room shouts out. ≥ 0.5 → DARK. |
| 42–45 min | **Debrief & bridge** | What we did *not* check today — and why that is exactly Session 8. |
| 45–60 min | **Q&A** | See `exercises/discussion.md`. |

**Honesty note on timing.** 45 minutes is enough *only* if the presenter's notebook is pre-written and the room is editing rather than typing from scratch. Budget the first four minutes ruthlessly for setup — a room where three people are still logging into Google at minute twelve loses the session. If something must be cut, compress the compile segment (16–21) to two minutes; **do not cut the honest moment (21–27)**, which is the pedagogical point of the whole hour.

## Materials & tools

- **Primary environment: Google Colab** (free tier, CPU is plenty). `tensorflow`, `pandas`, `numpy`, `scikit-learn`, `matplotlib` are all pre-installed — no `pip install`.
- **Fallback: JupyterLite is NOT usable for this session.** It runs Python in the browser with no login, but it has **no TensorFlow/Keras build** (`resources/sources.md` #7). Anyone blocked from Colab should pair with a colleague, or use Kaggle Notebooks (also free, also needs an account).
- **Second fallback (built into the lab): a self-contained dataset generator.** If the dataset URL is unreachable — corporate proxy, dead link, offline room — Cell 1b synthesises an equivalent RGB / light-dark dataset in six lines of NumPy. The lab then runs identically. **Nothing about this session depends on the network being up except one CSV fetch, and we have removed even that dependency.**
- **Dataset:** `https://tinyurl.com/y2qmhfsr` → `raw.githubusercontent.com/thomasnield/machine-learning-demo-data/.../light_dark_font_training_set.csv`. Confirmed resolving 2026-07-17. ⚠️ **Verify it resolves on the morning of delivery** — it is a 2024-era short link, and the lab prints a clear failure message plus the fallback if it does not.
- **Deliverable the participant keeps:** the notebook. Session 8 opens by reloading exactly this data and this workflow.

## Source & licence note

The idea of teaching deep learning through a light/dark-font colour classifier, and the `3 → 3 → 1` Keras build, come from **Thomas Nield's *Deep Learning for Beginners*, Day 1** (O'Reilly, 2024) — a commercial, all-rights-reserved deck, therefore **LINK-ONLY**. Nothing from it is reproduced. That deck's two exercise slides are **title-only** (no prompt, no code, no solution), so this entire lab is authored from scratch.

Everything placed on a slide is **SLIDE-SAFE**: our own code and prose, and the **TensorFlow/Keras API** (Apache-2.0). Two documented source defects are **corrected, not repeated** (`AI_input.md` §6):

- **Error #1 — the threshold contradiction.** The source says `≥ .5 → DARK` on one slide and `≥ .5 → light` on another. We use **≥ 0.5 → DARK**, because the output is defined as the probability of dark text. Held consistently across Sessions 6, 7, and 8.
- **Error #2 — the weight-initialisation claim.** The source states weights start "between −1 and 1" while its own code uses `np.random.rand`, which returns **0 to 1**. We neither repeat the claim nor hand-wave it: the lab **prints Keras's actual initial weights** and names the real default (Glorot-uniform, symmetric around zero, biases zero).

Full verdicts in [`resources/sources.md`](resources/sources.md).
