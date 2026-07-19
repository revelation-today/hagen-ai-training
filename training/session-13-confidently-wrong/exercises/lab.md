# Lab — Session 13: Make the Metric Lie, Then Catch It

**Time:** ~25–30 minutes · **Level:** no prior scikit-learn needed · **Language:** Python

This is an optional hands-on lab. **Nothing in Session 13 requires it** — the vendor role-play works entirely on paper. But if you build or evaluate models, or you will ever be handed a model evaluation report, doing this once makes the arithmetic yours instead of the presenter's.

By the end you will have: a confusion matrix and all six metrics for the Michael model; a reusable Bayes calculator; a prevalence sweep showing one unchanged test going from 1.6% to 79.8% precision; and a translation of the vendor scenario into your own team's numbers.

---

## Setup

**Colab (recommended).** Open `colab.research.google.com` → New notebook. `numpy` and `scikit-learn` are pre-installed; run `import sklearn; print(sklearn.__version__)` to confirm.

**JupyterLite fallback** (no account, runs entirely in your browser — use this if external notebook hosting is restricted): `jupyter.org/try-jupyter/lab/`. Then in the first cell:

```python
%pip install scikit-learn
```

**Local:** `pip install numpy scikit-learn`. Any version from the last few years works; nothing here uses a recent API.

> scikit-learn is **BSD-3 licensed** — free to use, modify and present with attribution.

---

## Part 1 — Build the Michael model (8 min)

### Step 1.1 — The data

100 employees over one year. Convention: **1 = "quits" is the positive class** (the event of interest), 0 = "stays".

```python
import numpy as np
from sklearn.metrics import (confusion_matrix, accuracy_score, precision_score,
                             recall_score, f1_score, classification_report)

# index 0      -> named Michael: model says QUIT, actually STAYED   (false positive)
# index 1      -> someone else:  model says STAY, actually QUIT     (false negative)
# indices 2-99 -> 98 people:     model says STAY, actually STAYED   (true negatives)

y_true = np.array([0, 1] + [0] * 98)   # what actually happened
y_pred = np.array([1, 0] + [0] * 98)   # what the model predicted

print(f"{y_true.sum()} people actually quit; the model predicted {y_pred.sum()} would.")
# 1 people actually quit; the model predicted 1 would.
```

Note that the model predicted the *right number* of quitters. It just picked the wrong person. No aggregate count would reveal this.

### Step 1.2 — The confusion matrix

```python
cm = confusion_matrix(y_true, y_pred, labels=[0, 1])
print(cm)
# [[98  1]
#  [ 1  0]]

tn, fp, fn, tp = cm.ravel()
print(f"TN={tn}  FP={fp}  FN={fn}  TP={tp}")
# TN=98  FP=1  FN=1  TP=0
```

**Read it before moving on.** scikit-learn puts **truth on the rows, predictions on the columns**. Row 1 (`[1, 0]`) is the people who actually quit: 1 missed, 0 caught. Column 1 is everyone the model flagged: 1 wrong, 0 right.

### Step 1.3 — All six metrics

```python
sensitivity = recall_score(y_true, y_pred, zero_division=0)      # TP/(TP+FN)
precision   = precision_score(y_true, y_pred, zero_division=0)   # TP/(TP+FP)
f1          = f1_score(y_true, y_pred, zero_division=0)
accuracy    = accuracy_score(y_true, y_pred)
specificity = tn / (tn + fp)          # no sklearn one-liner for these two
npv         = tn / (tn + fn)

for name, value in [("Sensitivity (recall)", sensitivity),
                    ("Specificity", specificity),
                    ("Precision", precision),
                    ("Neg. predictive value", npv),
                    ("Accuracy", accuracy),
                    ("F1 score", f1)]:
    print(f"{name:<24} {value:.4f}")

# Sensitivity (recall)     0.0000
# Specificity              0.9899
# Precision                0.0000
# Neg. predictive value    0.9899
# Accuracy                 0.9800
# F1 score                 0.0000   <-- mathematically UNDEFINED (0/0)
```

Now see what the library does without `zero_division`:

```python
import warnings
with warnings.catch_warnings(record=True) as caught:
    warnings.simplefilter("always")
    f1_raw = f1_score(y_true, y_pred)
    print(f1_raw, [str(w.category.__name__) for w in caught])
# 0.0 ['UndefinedMetricWarning']
```

**The lesson.** F1 here is 0/0 — undefined. The library returns `0.0` and emits a warning that most pipelines suppress and most dashboards never surface. **If you see F1 = 0.00 in a report, it may mean "scored badly" or it may mean "made zero correct positive predictions."** Those are different diagnoses and only one is fixable by tuning.

### Step 1.4 — The full report, and the numbers nobody reads

```python
print(classification_report(y_true, y_pred,
                            target_names=["stays (0)", "quits (1)"],
                            zero_division=0))
#               precision    recall  f1-score   support
#
#    stays (0)       0.99      0.99      0.99        99
#    quits (1)       0.00      0.00      0.00         1
#
#     accuracy                           0.98       100
#    macro avg       0.49      0.49      0.49       100
# weighted avg       0.98      0.98      0.98       100
```

Three under-read numbers:

- **`support` = 1** for the positive class. Any metric computed on one example is noise, not evidence. This is the first column to check and the one everyone skips.
- **`macro avg` 0.49** vs. **`weighted avg` 0.98.** Macro treats both classes as equally important; weighted lets the 99 boring cases drown the 1 interesting one. **0.49 is the honest summary; 0.98 is the number that goes in the deck.**

### Step 1.5 — The degenerate baseline

```python
# "Predict nobody ever quits" -- no model, no features, no data.
y_true_year = np.array([1] + [0] * 99)   # 1 in 100 quits
y_pred_lazy = np.zeros(100, dtype=int)

print(accuracy_score(y_true_year, y_pred_lazy))
# 0.99
```

**99% accurate, from a constant.** Compute this baseline for every classifier you are ever shown. If the real model does not beat it on *precision and recall*, there is no model.

---

## Part 2 — The Bayes calculator (8 min)

### Step 2.1 — The function

```python
def posterior(sensitivity, specificity, prevalence):
    """P(condition | positive result) -- the precision you will actually
    experience in a population with the given prevalence.

    sensitivity -- P(positive | condition present)   <- what vendors quote
    specificity -- P(negative | condition absent)    <- ask for this
    prevalence  -- P(condition) in YOUR population   <- find this yourself
    Returns (precision, overall_positive_rate).
    """
    true_pos  = sensitivity * prevalence
    false_pos = (1 - specificity) * (1 - prevalence)
    p_positive = true_pos + false_pos
    return true_pos / p_positive, p_positive
```

### Step 2.2 — Validate it against the vendor's own matrix

Never trust a model of a test until it reproduces the test's published numbers.

```python
# Vendor's confusion matrix over 1,000 patients:
#              positive  negative
#   at risk        198        2     -> sensitivity = 198/200
#   not at risk     50      750     -> specificity = 750/800
SENS = 198 / 200      # 0.99
SPEC = 750 / 800      # 0.9375

prec, rate = posterior(SENS, SPEC, prevalence=0.20)   # their sample was 20% at risk
print(f"precision {prec:.4f}   positive rate {rate:.4f}")
# precision 0.7984   positive rate 0.2480

print(198 / 248, 248 / 1000)
# 0.7983870967741935 0.248
```

**They match.** 79.8% precision and a 24.8% positive rate, reproduced from sensitivity and specificity alone. The model of the test is correct — so nobody can dismiss the next step as an artefact.

### Step 2.3 — Change one argument

```python
prec_real, rate_real = posterior(SENS, SPEC, prevalence=0.01)   # the real world
print(f"precision {prec_real:.1%}   positive rate {rate_real:.1%}")
# precision 13.8%   positive rate 7.2%
```

**79.8% → 13.8%.** One keyword argument. The test did not change.

### Step 2.4 — Count the bodies, to prove it to yourself

```python
N = 100_000
at_risk = int(N * 0.01)                    # 1,000
healthy = N - at_risk                       # 99,000
tp = round(at_risk * SENS)                  # 990
fn = at_risk - tp                           # 10
fp = round(healthy * (1 - SPEC))            # 6,188 (6187.5 rounded)
tn = healthy - fp                           # 92,812

print(f"flagged: {tp+fp}   of which wrong: {fp}   precision: {tp/(tp+fp):.1%}")
# flagged: 7178   of which wrong: 6188   precision: 13.8%
```

Six of every seven positives are false alarms. **Always be able to reach a Bayes result by counting bodies** — it is the version you can defend in a meeting, and it is immune to the denominator error the source deck made.

### Step 2.5 — The prevalence sweep

```python
print(f"{'prevalence':>10} | {'% flagged':>9} | {'precision':>9}")
print("-" * 34)
for prev in [0.001, 0.005, 0.01, 0.02, 0.05, 0.10, 0.20]:
    p, r = posterior(SENS, SPEC, prev)
    print(f"{prev:>9.1%} | {r:>8.1%} | {p:>8.1%}")

# prevalence | % flagged | precision
# ----------------------------------
#      0.1% |     6.3% |     1.6%
#      0.5% |     6.7% |     7.4%
#      1.0% |     7.2% |    13.8%
#      2.0% |     8.1% |    24.4%
#      5.0% |    10.9% |    45.5%
#     10.0% |    15.5% |    63.8%
#     20.0% |    24.8% |    79.8%
```

**Read the middle column.** The flag rate moves fourfold while precision moves fiftyfold. **Alert volume tells you almost nothing about alert quality** — a steady alert count is fully compatible with a tool that has become worthless.

### Step 2.6 — Verify the correction

```python
deck_figure = 0.99 * 0.01 / 0.248     # the source's denominator
print(f"{deck_figure:.4%}")            # 3.9919%  -- the source prints 3.39%
print(f"{posterior(SENS, SPEC, 0.01)[0]:.4%}")   # 13.7933%
```

Two problems, both visible in three lines: the source's arithmetic gives 3.99% not 3.39%, **and** its denominator (0.248) is the positive rate in the vendor's 20%-prevalence sample, not in a 1% population. Recomputing the denominator honestly gives **13.79%**. Same verdict.

---

## Part 3 — Your own numbers (8 min)

Replace the medical vendor with a tool your team might actually buy.

```python
# An AI defect-prediction tool.
# Vendor benchmark: 1,000 commits, 200 of them containing an escaping defect.
#   flagged 198 of the 200 defective  -> sensitivity 0.99
#   flagged  50 of the 800 clean      -> specificity 0.9375
SENS = 0.99
SPEC = 0.9375

YOUR_BASE_RATE = 0.01      # <-- CHANGE ME. What fraction of YOUR commits
                           #     produce a defect that escapes to the field?
COMMITS = 10_000           # <-- CHANGE ME. A quarter's work.
MINUTES_PER_INVESTIGATION = 20

prec, rate = posterior(SENS, SPEC, YOUR_BASE_RATE)
flagged   = COMMITS * rate
real      = COMMITS * YOUR_BASE_RATE * SENS
false     = flagged - real
hours     = flagged * MINUTES_PER_INVESTIGATION / 60

print(f"flagged commits ....... {flagged:>8.0f}")
print(f"  really defective .... {real:>8.0f}")
print(f"  false alarms ........ {false:>8.0f}  ({1-prec:.0%})")
print(f"investigation effort .. {hours:>8.0f} hours  "
      f"({hours/(13*5*8)*100:.0f}% of one engineer's quarter)")
print(f"defects missed ........ {COMMITS*YOUR_BASE_RATE*(1-SENS):>8.0f}")

# flagged commits ....... 718
#   really defective .... 99
#   false alarms ........ 619  (86%)
# investigation effort .. 239 hours  (46% of one engineer's quarter)
# defects missed ........ 1
```

**Now answer three questions in writing:**

1. Would your team sustain 11 investigations per working day, 86% of which find nothing? For how many weeks?
2. When they stop reading the flags — and they will — what in the tool's own reporting would tell you that has happened? *(Answer: nothing. Sensitivity stays at 99%, truthfully, forever.)*
3. What base rate would make this tool worth buying, and can you narrow the deployment population to reach it? (Try `YOUR_BASE_RATE = 0.05` — precision jumps to 45.5%. What subset of commits has a 5% escape rate? High-churn modules? Changes touching the release branch? First-time contributors?)

That third question is the constructive one. **Narrowing the population is almost always cheaper than improving the model** — and it is a decision you control, while the model is not.

---

## Now break it / now extend it

1. **Break the F1.** Change `y_pred` so the model correctly identifies the one quitter (`y_pred = np.array([0, 1] + [0]*98)`). Recompute all six metrics. Accuracy goes from 0.98 to **1.00** — a 2-point move — while precision, recall and F1 go from 0/0/undefined to **1.0/1.0/1.0**. One person changed. Which metric told you the model went from useless to perfect? Which one barely moved? *This is the strongest single demonstration in the lab — do it if you do nothing else.*

2. **Find the specificity you actually need.** At a 1% base rate and 99% sensitivity, solve for the specificity that gives 50% precision, then 90%. (Answers: **99%** and **99.89%**.) Write a loop that searches for it. Then consider what it means that a rare-event tool needs a false-positive rate below one in a thousand — and why "the vendor will improve the model" is usually not a plan.

3. **Reverse the vendor's incentive.** Suppose the vendor is measured on the headline number they can advertise. Given free choice of their validation sample's composition, what prevalence would they choose, and what would they report? Now re-read the prevalence sweep as *the vendor's* menu rather than yours.

4. **Simulate the human control.** Model the 99% trap from `content/02`: give a reviewer a detection probability that falls as the model's error rate falls (e.g. `p_detect = 0.9 * error_rate ** 0.4`), and compute errors reaching production across model accuracies from 90% to 99.9%. Find the accuracy at which further improvement stops helping — or starts hurting. Then ask what your organisation would have to measure to notice.

5. **Threshold sweep (if you did Session 8's lab).** Take a real classifier, vary the decision threshold from 0.05 to 0.95, and plot precision and recall against it. Watch them trade off. Then apply `posterior()` at two different deployment base rates and see the whole precision curve shift while the recall curve does not move at all. That asymmetry *is* the session.

---

## What to take away from the lab

- The confusion matrix is four numbers, and every metric is one cell over a row or column total. **Which total you divide by is the entire question.**
- `support`, `macro avg`, and the degenerate baseline are the three numbers that reveal a fake result, and the three nobody reads.
- Sensitivity and specificity travel with the test. **Precision does not** — it is a joint property of the test and the population.
- Eleven lines of Python convert any vendor's quoted sensitivity into the precision you will experience. Keep `posterior()` somewhere you can find it before your next vendor meeting.
