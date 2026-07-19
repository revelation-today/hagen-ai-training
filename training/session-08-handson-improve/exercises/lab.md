# Lab — Make It Better: Overfit It, Fix It, Measure It

**Time:** ~25–30 minutes · **Environment:** Google Colab (free tier, CPU is fine — no GPU needed) · **Prereq:** Session 7's Keras build.

This lab continues Session 7's network. You will deliberately **overfit** it, **fix** it three ways, **tune** its knobs, then switch to a second dataset and learn to read a **confusion matrix** with precision and recall. Every code cell shows expected output in comments — yours will differ slightly because neural-network training is random (we set seeds to reduce, not eliminate, this).

## Setup

**Colab:** open [colab.research.google.com](https://colab.research.google.com) → *New notebook*. TensorFlow, scikit-learn, numpy, pandas, and matplotlib are pre-installed. No `pip install` needed.

**JupyterLite fallback — partial only:** JupyterLite (browser-only, no login) runs the `sklearn.metrics` parts (Part 5) but **cannot run TensorFlow/Keras** (Parts 1–4). If you're blocked from Colab, do Part 5 in JupyterLite and watch Parts 1–4. See `resources/sources.md`.

```python
# Cell 0 — imports and reproducibility
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.callbacks import EarlyStopping
from sklearn.model_selection import train_test_split

tf.random.set_seed(42)
np.random.seed(42)
print("TensorFlow", tf.__version__)
# Expected: TensorFlow 2.x.x
```

---

## Part 1 — Load the Session 7 data and force an overfit (≈6 min)

We reload the RGB colour dataset from Session 7 (predict light vs. dark font for a background colour), then deliberately make the model memorise.

```python
# Cell 1 — load the colour dataset (same as Session 7)
URL = "https://raw.githubusercontent.com/thomasnield/machine-learning-demo-data/master/classification/light_dark_font_training_set.csv"
df = pd.read_csv(URL)
print(df.head(2))
print("rows:", len(df))
# Expected columns: RED, GREEN, BLUE, LIGHT_OR_DARK_FONT_IND
# rows: ~1300+  (exact count may vary; the schema is what matters)

X = df[["RED", "GREEN", "BLUE"]].values / 255.0   # scale to 0..1 (known range)
y = df["LIGHT_OR_DARK_FONT_IND"].values           # 0/1 label
```

Now split off a test set, then **cripple the training set down to 60 rows** — the first ingredient of overfitting (too little data).

```python
# Cell 2 — split, then shrink training data to force overfitting
X_train_full, X_test, y_train_full, y_test = train_test_split(
    X, y, test_size=0.25, stratify=y, random_state=42)

# TAKE ONLY 60 TRAINING ROWS on purpose:
X_small, y_small = X_train_full[:60], y_train_full[:60]
print("training rows now:", len(X_small), "| test rows:", len(X_test))
# Expected: training rows now: 60 | test rows: ~325
```

Build an **oversized** network (ingredient two) and train for **many epochs** (ingredient three):

```python
# Cell 3 — an intentionally huge network, trained too long, on too little data
def build_big():
    m = Sequential([
        Dense(256, activation="relu", input_shape=(3,)),
        Dense(256, activation="relu"),
        Dense(1, activation="sigmoid"),
    ])
    m.compile(optimizer=Adam(1e-3), loss="binary_crossentropy", metrics=["accuracy"])
    return m

overfit_model = build_big()
h = overfit_model.fit(X_small, y_small,
                      validation_data=(X_test, y_test),
                      epochs=300, batch_size=8, verbose=0)

print("final TRAIN acc:", round(h.history["accuracy"][-1], 3))
print("final VAL   acc:", round(h.history["val_accuracy"][-1], 3))
# Example output (yours will vary):
# final TRAIN acc: 1.0        <- memorised all 60 rows
# final VAL   acc: 0.9        <- new data: noticeably worse => OVERFITTING
```

> **Note on loss:** we use `binary_crossentropy`, not the `mean_squared_error` the original source deck used with a sigmoid. Binary cross-entropy is the conventional, better-behaved choice for a 0/1 classifier (see `content/03`).

**Plot the two curves — this is the money shot of the whole session:**

```python
# Cell 4 — SEE the overfit: train and validation split apart
def plot_history(h, title):
    plt.figure(figsize=(6,4))
    plt.plot(h.history["accuracy"], label="train accuracy")
    plt.plot(h.history["val_accuracy"], label="validation accuracy")
    plt.title(title); plt.xlabel("epoch"); plt.ylabel("accuracy")
    plt.legend(); plt.grid(True, alpha=0.3); plt.show()

plot_history(h, "Forced overfit: the two lines split apart")
# Expected shape: 'train accuracy' climbs to ~1.0 and stays;
# 'validation accuracy' rises then FLATTENS well below it (and may drift down).
# The vertical GAP between the lines IS the overfitting.
```

**Debrief:** Point at the gap. That gap is not a bug in your code — it is the model memorising 60 rows instead of learning the pattern. Everything in Part 2 shrinks that gap.

---

## Part 2 — Fix the overfit three ways (≈7 min)

We apply the three remedies from `content/02`, one at a time, and re-measure the gap each time.

### Fix 1 — more data

```python
# Cell 5 — same huge model, but the FULL training set instead of 60 rows
fix_data = build_big()
h_data = fix_data.fit(X_train_full, y_train_full,
                      validation_data=(X_test, y_test),
                      epochs=300, batch_size=32, verbose=0)
print("more-data  TRAIN acc:", round(h_data.history["accuracy"][-1], 3),
      "| VAL acc:", round(h_data.history["val_accuracy"][-1], 3))
# Example: more-data TRAIN acc: 0.97 | VAL acc: 0.96  <- gap almost gone
```

### Fix 2 — dropout (regularise capacity when you can't add data)

```python
# Cell 6 — back to only 60 rows, but add Dropout to fight memorisation
def build_dropout():
    m = Sequential([
        Dense(256, activation="relu", input_shape=(3,)),
        Dropout(0.4),
        Dense(256, activation="relu"),
        Dropout(0.4),
        Dense(1, activation="sigmoid"),
    ])
    m.compile(optimizer=Adam(1e-3), loss="binary_crossentropy", metrics=["accuracy"])
    return m

fix_drop = build_dropout()
h_drop = fix_drop.fit(X_small, y_small,          # STILL only 60 rows
                      validation_data=(X_test, y_test),
                      epochs=300, batch_size=8, verbose=0)
print("dropout    TRAIN acc:", round(h_drop.history["accuracy"][-1], 3),
      "| VAL acc:", round(h_drop.history["val_accuracy"][-1], 3))
# Example: dropout TRAIN acc: 0.88 | VAL acc: 0.90
# Note: train acc is now LOWER than val acc — that's dropout working as intended
# (it handicaps training on purpose; it's off during evaluation). Smaller gap.
```

### Fix 3 — early stopping (stop at the validation-loss minimum)

```python
# Cell 7 — 60 rows again, huge model, but stop when validation stops improving
early = EarlyStopping(monitor="val_loss", patience=15, restore_best_weights=True)
fix_early = build_big()
h_early = fix_early.fit(X_small, y_small,
                        validation_data=(X_test, y_test),
                        epochs=300, batch_size=8, callbacks=[early], verbose=0)
print("stopped after", len(h_early.history["loss"]), "epochs (not 300)")
print("early-stop TRAIN acc:", round(h_early.history["accuracy"][-1], 3),
      "| VAL acc:", round(h_early.history["val_accuracy"][-1], 3))
# Example:
# stopped after ~90 epochs (not 300)
# early-stop TRAIN acc: 0.95 | VAL acc: 0.91  <- stopped before it memorised
```

**Compare all four side by side:**

```python
# Cell 8 — the scoreboard
def gap(h): return round(h.history["accuracy"][-1] - h.history["val_accuracy"][-1], 3)
print(f"{'run':<22}{'train':>7}{'val':>7}{'gap':>7}")
for name, hh in [("forced overfit", h), ("+ more data", h_data),
                 ("+ dropout", h_drop), ("+ early stopping", h_early)]:
    print(f"{name:<22}{hh.history['accuracy'][-1]:>7.2f}"
          f"{hh.history['val_accuracy'][-1]:>7.2f}{gap(hh):>7.2f}")
# Example:
# run                     train    val    gap
# forced overfit           1.00   0.90   0.10
# + more data              0.97   0.96   0.01
# + dropout                0.88   0.90  -0.02
# + early stopping         0.95   0.91   0.04
```

**Debrief:** every fix shrank the gap. More data was strongest (it's the real cure); dropout even pushed val *above* train; early stopping cost the least effort. In real work you often combine them.

---

## Part 3 — Tune the knobs (≈5 min)

Sweep the learning rate, holding everything else fixed — the honest tuning loop from `content/03`.

```python
# Cell 9 — learning-rate sweep on the full data
def build_small(lr):
    m = Sequential([Dense(8, activation="relu", input_shape=(3,)),
                    Dense(1, activation="sigmoid")])
    m.compile(optimizer=Adam(lr), loss="binary_crossentropy", metrics=["accuracy"])
    return m

for lr in [1.0, 1e-1, 1e-2, 1e-3, 1e-4]:
    m = build_small(lr)
    hh = m.fit(X_train_full, y_train_full, validation_data=(X_test, y_test),
               epochs=60, batch_size=32, verbose=0)
    print(f"lr={lr:<7} -> val_acc={hh.history['val_accuracy'][-1]:.3f}")
# Example output (illustrative):
# lr=1.0     -> val_acc=0.71    (too high: unstable, mediocre)
# lr=0.1     -> val_acc=0.94
# lr=0.01    -> val_acc=0.96    (good)
# lr=0.001   -> val_acc=0.96    (good)
# lr=0.0001  -> val_acc=0.88    (too low for only 60 epochs: underfit)
```

**Debrief:** there is a failure mode at *each* end — too high is unstable, too low is too slow. The "best" learning rate is found by watching the validation column, not by faith. (Try the network-size sweep in the challenges.)

---

## Part 4 — A clean, tuned model (≈2 min)

Put the lessons together: right-sized network, full data, early stopping. This is the model we'll measure in Part 5's *style* (we keep the colour model here; Part 5 switches datasets to get real stakes).

```python
# Cell 10 — the "good practice" colour model
good = Sequential([Dense(8, activation="relu", input_shape=(3,)),
                   Dense(1, activation="sigmoid")])
good.compile(optimizer=Adam(1e-2), loss="binary_crossentropy", metrics=["accuracy"])
good.fit(X_train_full, y_train_full, validation_data=(X_test, y_test),
         epochs=200, batch_size=32,
         callbacks=[EarlyStopping(monitor="val_loss", patience=15,
                                  restore_best_weights=True)], verbose=0)
print("clean model VAL acc:", round(good.evaluate(X_test, y_test, verbose=0)[1], 3))
# Example: clean model VAL acc: 0.965
```

---

## Part 5 — Does it actually work? Confusion matrix on a second dataset (≈8 min)

The colour problem has no stakes (a wrong font colour is cosmetic), so accuracy tells you all you need. To learn to *read* a model, we switch to a dataset where the errors matter: scikit-learn's bundled **breast-cancer** data (569 tumours, 30 measurements each, malignant vs. benign — ships with the library, no download). **The workflow is identical; only the stakes change.**

```python
# Cell 11 — new dataset, SAME workflow
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler

data = load_breast_cancer()
Xb, yb = data.data, data.target          # Xb: (569, 30); yb: 0=malignant, 1=benign
print("class balance (0=malignant,1=benign):", np.bincount(yb))
# Expected: class balance: [212 357]

Xb_tr, Xb_te, yb_tr, yb_te = train_test_split(
    Xb, yb, test_size=0.2, stratify=yb, random_state=42)

scaler = StandardScaler().fit(Xb_tr)     # fit on TRAIN ONLY (never on test)
Xb_tr, Xb_te = scaler.transform(Xb_tr), scaler.transform(Xb_te)

cancer = Sequential([Dense(16, activation="relu", input_shape=(30,)),   # only change: 30 inputs
                     Dropout(0.3),
                     Dense(1, activation="sigmoid")])
cancer.compile(optimizer=Adam(1e-3), loss="binary_crossentropy", metrics=["accuracy"])
cancer.fit(Xb_tr, yb_tr, validation_split=0.2, epochs=200, batch_size=16,
           callbacks=[EarlyStopping(monitor="val_loss", patience=20,
                                    restore_best_weights=True)], verbose=0)
print("test accuracy:", round(cancer.evaluate(Xb_te, yb_te, verbose=0)[1], 3))
# Example: test accuracy: 0.956   <- looks great. But is it? Read on.
```

Now the point of the whole session — **look past the accuracy at the confusion matrix:**

```python
# Cell 12 — confusion matrix + precision / recall / F1
from sklearn.metrics import (confusion_matrix, classification_report,
                             ConfusionMatrixDisplay)

probs = cancer.predict(Xb_te).ravel()
preds = (probs >= 0.5).astype(int)

cm = confusion_matrix(yb_te, preds)
print(cm)
# Example (rows=actual, cols=predicted; 0=malignant, 1=benign):
# [[40  3]     <- 43 malignant: 40 caught, 3 MISSED (false negatives)
#  [ 2 69]]    <- 71 benign:    2 false alarms, 69 cleared

print(classification_report(yb_te, preds, target_names=["malignant", "benign"]))
# Example (illustrative):
#               precision    recall  f1-score   support
#    malignant       0.95      0.93      0.94        43
#       benign       0.96      0.97      0.96        71
#     accuracy                           0.96       114

ConfusionMatrixDisplay(cm, display_labels=["malignant","benign"]).plot()
plt.show()
```

**Read it like a professional:**

```python
# Cell 13 — the number that actually matters here
tn, fp, fn, tp = confusion_matrix(yb_te, preds, labels=[1,0]).ravel()
# NOTE labels=[1,0] so that MALIGNANT (0) is treated as the POSITIVE class.
recall_malignant = tp / (tp + fn)
precision_malignant = tp / (tp + fp)
print(f"malignant recall (caught / actual):   {recall_malignant:.3f}  "
      f"<- we MISSED {fn} cancers")
print(f"malignant precision (right / flagged): {precision_malignant:.3f}")
# Example:
# malignant recall (caught / actual):   0.930  <- we MISSED 3 cancers
# malignant precision (right / flagged): 0.952
```

**Debrief — the payoff of the session:** 96% accuracy looked excellent, but the confusion matrix reveals the model **missed 3 of 43 cancers** (recall 0.93). Whether that is acceptable is a *decision*, not a number — and no accuracy figure could have told you. This is exactly the instinct Session 13 weaponises against a vendor's "99% accurate" claim.

> **The silent trap, made explicit:** in this dataset `0 = malignant`, so the "positive" class you care about is class 0, *not* class 1. `recall_score(yb_te, preds)` with no `pos_label` would report recall for *benign* — the wrong number. Always confirm which class is positive before trusting any precision/recall value.

---

## Now break it / now extend it

**Break it:**

1. **Remove early stopping** from Cell 10 and train the clean colour model for 1000 epochs. Plot the history. Does validation loss turn back up? Where's the U's minimum?
2. **Set the learning rate to `5.0`** in Cell 9's model. What does the loss do? (Look for `NaN` — the classic "too high" signature.)
3. **Move the decision threshold** in Cell 12 from `0.5` to `0.30`, then to `0.70`. Recompute the confusion matrix each time. Watch malignant recall and precision trade off. Which threshold would you choose for a cancer screen, and why?

**Extend it:**

4. **Network-size sweep:** copy Cell 9 but vary the hidden layer over `[2, 8, 32, 256]` units at a fixed good learning rate. Find where it stops underfitting and starts overfitting.
5. **Combine all three fixes** on the 60-row colour data: full-ish data + dropout + early stopping together. Can you beat every single-fix run in Cell 8's scoreboard?
6. **Try a third dataset** the room chooses — e.g. `from sklearn.datasets import load_wine` (multi-class: change the final layer to `Dense(3, activation="softmax")` and the loss to `"sparse_categorical_crossentropy"`). Prove the seven-step workflow still holds. Read the *multi-class* confusion matrix — now it's a 3×3 grid.
7. **Plot the ROC curve** for the breast-cancer model: `from sklearn.metrics import RocCurveDisplay; RocCurveDisplay.from_predictions(yb_te, probs)`. This sweeps every threshold at once — the natural sequel to Cell 13's single-threshold view.

## What to keep

Save this notebook. It is a **template**: load → scale → split → build → fit-with-early-stopping → confusion-matrix. Swap the dataset in Cell 11 and you have a starting point for real work. The seven steps do not change; only the stakes and the judgement calls do.
