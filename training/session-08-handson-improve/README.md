# Session 8 — Hands-On II: Make It Better

**Block:** Do · **Goal 4 (second half)** · **Format:** hands-on lab (the lab is the session) · 45 min + 15 min Q&A

---

## One-paragraph summary

In Session 7 the room trained a small neural network and watched its accuracy climb — "it trains." This session is about the harder, more honest half of the job: **turning a model that trains into a model that is actually good**, and knowing the difference. We make **overfitting visible** (train accuracy near-perfect, test accuracy stuck lower) and fix it three ways (more data, dropout, early stopping); we **tune the knobs that matter** (learning rate, epochs, network size) and watch each one help or hurt; and we learn to **read a confusion matrix and precision/recall** so we can tell whether a model works rather than trusting a single flattering number. We then re-run the whole workflow on a second, unrelated dataset to prove it transfers. This is the session where "machine learning" starts to look like engineering — and it deliberately sets up Session 13 ("your metric is lying").

## Audience & level

Qualcomm release / problem / configuration managers and developers who completed Session 7 (or can build and `fit()` a small Keras model). **Managers:** you do not need to write the code to get the payoff — the reading-a-metric material (`content/04`) is role-critical, because you will be handed accuracy numbers by vendors and teams and must know what they hide. **Developers:** the lab is type-along; you leave with a notebook you can reuse on your own data.

## Learning objectives

By the end, a participant can:

1. **Diagnose** overfitting from a train-vs-validation curve (the gap, and when it opens).
2. **Apply** three overfitting remedies in Keras — more data, `Dropout`, and `EarlyStopping` — and measure whether each helped.
3. **Tune** learning rate, epochs, and network size, and explain the failure mode at each extreme.
4. **Compute and read** a confusion matrix with `sklearn.metrics`, and derive precision, recall, specificity, and F1 from it by hand.
5. **Explain** why accuracy alone is misleading under class imbalance, and which metric matters for a given cost of error (false negative vs. false positive).
6. **Transfer** the entire workflow to a new dataset with no code rewritten in anger.

## Prerequisites

- **Session 7** — building, compiling, and `fit()`-ing a `Sequential` model; the `/255` scaling; `Dense`, `relu`, `sigmoid`, `epochs`, `batch_size`.
- **Session 3** (train/test split, generalisation) and **Session 6** (what a neural network is, conceptually) are assumed but not required.
- A Google account for Colab. No local install. (JupyterLite fallback is **not** available for this session — see Materials.)

## Agenda (45 min delivery + 15 min Q&A)

This is a lab. The presenter drives a Colab notebook; the room types along. Each segment is *do it, then debrief*.

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Recap & hook** | Session 7's model "trains." Question: is a high training accuracy good news? |
| 3–11 min | **Overfitting made visible** | Shrink the data, grow the net, over-train → watch train accuracy hit ~100% while validation stalls. Read the diverging curve. |
| 11–20 min | **Fix it three ways** | More data → `Dropout` → `EarlyStopping`. Re-plot after each; the gap closes. |
| 20–29 min | **Tune the knobs** | Learning rate (too high / too low), epochs, network size. Watch each help or hurt. |
| 29–41 min | **Does it actually work?** | Switch to a second dataset (breast-cancer). Confusion matrix, precision, recall, F1 — by hand and via `sklearn.metrics`. Move the threshold. |
| 41–45 min | **Debrief & bridge** | "Accuracy is a headline, not the story." Set up Session 13. |
| 45–60 min | **Q&A** | See `exercises/discussion.md`. |

**Honesty note on timing:** this is tight. The lab (`exercises/lab.md`) is a self-contained ~25–30 min run; in the live session the presenter runs the pre-filled notebook and the room edits one cell at a time, rather than typing everything. If a segment runs long, the tuning segment (20–29) is the one to compress — the confusion-matrix segment is the payoff and must not be cut.

## Materials & tools

- **Primary environment:** Google Colab (free tier; CPU is fine — these models are tiny, no GPU needed). See `exercises/lab.md` for the notebook.
- **Libraries:** `tensorflow.keras`, `scikit-learn` (`sklearn.metrics`, bundled datasets), `numpy`, `matplotlib`, `pandas`.
- **Datasets:** the Session 7 colour dataset (RGB → light/dark font label) for the overfitting/tuning half; scikit-learn's bundled **breast-cancer** dataset (BSD-3, ships with the library, no download) for the metrics half.
- **No JupyterLite fallback:** JupyterLite runs scikit-learn but **not** TensorFlow/Keras (no WASM build) — so the Keras half cannot run there. If a participant is blocked from Colab, they can still do the `sklearn.metrics` half in JupyterLite; the Keras half must be watched. (See `resources/sources.md`.)

## Source & licence note

The topic set (overfitting, confusion matrix, precision/recall, ROC/AUC) is taken from **DL for Beginners, Day 3 §Testing & Validation** (Thomas Nield / O'Reilly) — that deck is **all-rights-reserved: LINK-ONLY**. Nothing from it is copied; concepts are re-taught in our own words, the "Michael" accuracy parable is paraphrased with a new framing, and the source's dead **Katacoda** labs are replaced with fresh Colab code we wrote.

Everything built onto slides is **SLIDE-SAFE**: our own code and prose, plus **scikit-learn** (BSD-3 — figures, the `metrics` API, and the bundled breast-cancer dataset) and **TensorFlow/Keras** (Apache-2.0 — API surface). Full verdicts in [`resources/sources.md`](resources/sources.md).
