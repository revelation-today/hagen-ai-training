# Key Takeaways — Build and Train a Network in Keras

A one-page recap. If a colleague missed the session, this plus the lab is what they need.

## The whole workflow, in eight arrows

> **load → scale → split → build → compile → fit → evaluate → predict**

Every Keras program you will ever write follows this. Swap the dataset and the input shape; the outline does not change.

## The code, complete

```python
# 1-3. load, scale, split
df = pd.read_csv("https://tinyurl.com/y2qmhfsr")          # verify at delivery
X  = df[["RED","GREEN","BLUE"]].values / 255.0
y  = df["LIGHT_OR_DARK_FONT_IND"].values
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=1/3, stratify=y, random_state=42)

# 4. build
model = tf.keras.Sequential([
    tf.keras.layers.Input(shape=(3,)),
    tf.keras.layers.Dense(3, activation="relu"),
    tf.keras.layers.Dense(1, activation="sigmoid"),
])

# 5. compile
model.compile(optimizer="adam", loss="binary_crossentropy", metrics=["accuracy"])

# 6. the honest moment, then fit
print("untrained:", model.evaluate(X_test, y_test, verbose=0)[1])   # ~0.55
model.fit(X_train, y_train, epochs=100, batch_size=32)

# 7-8. evaluate and predict
print("trained:  ", model.evaluate(X_test, y_test, verbose=0)[1])   # ~0.96
p = model.predict(np.array([[255,255,204]]) / 255.0)[0][0]          # ~0.98
print("DARK" if p >= 0.5 else "LIGHT")
```

## The vocabulary, settled

| Term | The one-line version |
|---|---|
| `Sequential` | A straight stack of layers, no branches. |
| `Dense(n)` | A fully connected layer of `n` neurons — every input touches every neuron. |
| `relu` | `max(0, z)`. The hidden-layer default: cheap, and gradients keep flowing. |
| `sigmoid` | Squashes any number into 0–1. Turns a score into a probability. Binary output layer. |
| `/255` | Scale RGB into 0–1 so inputs and weights start on comparable footing. |
| **loss** | The smooth number training minimises. `binary_crossentropy` for a 0/1 classifier. |
| **optimizer** | The rule for how far to move each weight. `adam` is the sensible default. |
| **metric** | A human-readable score, reported but never optimised. |
| **epoch** | One complete pass over the training data. |
| **batch_size** | Rows processed before each weight update. `updates = epochs × rows/batch_size`. |
| **parameter** | A number training chooses. We have 16. |
| **hyperparameter** | A number *you* choose — epochs, batch size, learning rate, layer sizes. |

## The eight things to actually remember

1. **A neural network is fifteen lines of code.** Ours has **16 parameters** — 9 weights + 3 biases into the hidden layer, 3 + 1 into the output. You can print all of them. A large language model is this structure with a hundred billion of them; the mechanism is identical.
2. **Build → compile → fit → evaluate.** Building sets the *shape*; compiling sets the *learning rules*; fitting is where weights actually change; evaluating on unseen data is the only honest score.
3. **Scale your inputs.** `/255` because 255 is a known maximum. When the range is unknown, fit a scaler on the training set only — never on data that includes your test set.
4. **The untrained model scored ~0.55 — and predicted the same class for every colour.** It wasn't making bad decisions; it wasn't making decisions. Its accuracy was just the majority class's share of the test set.
5. **Therefore: an accuracy number alone cannot tell you whether a model learned anything.** On a dataset that is 99% one class, doing nothing scores 99%. You watched the small version of this happen in your own notebook.
6. **Loss and accuracy are different tools.** Loss is smooth so the optimiser can follow its slope; accuracy is a step function with no usable slope. We minimise the proxy and watch the thing we care about.
7. **Change one thing, re-run, compare — and rebuild first.** Calling `fit()` again continues training rather than restarting. One run is a sample, not a measurement.
8. **Training the model is the easy part.** Twenty-five minutes to a 96% classifier. Everything genuinely hard — data quality, label direction, which errors it makes, whether the score means anything, what happens in production — is downstream of `fit()`.

## Honest caveats we kept in view

- **This problem does not need a neural network.** Light-vs-dark is essentially a brightness calculation; a rule or a logistic regression solves it. We used a network because it is small enough to see through. In the extension exercises, replacing `relu` with a linear activation often works *just as well* — which is the honest evidence for exactly this point.
- **Our network is not deep learning.** "Deep" means more than one hidden layer; ours has one. Adding a second layer (challenge 5) does not improve it, because there is no complicated boundary here to represent.
- **Two source-deck errors were corrected, not repeated.** The threshold contradiction — we hold **≥ 0.5 → DARK** everywhere. And the "weights initialise between −1 and 1" claim, which its own code contradicts (`np.random.rand` gives 0 to 1); instead of repeating either version, the lab prints Keras's real defaults — Glorot-uniform weights symmetric around zero, biases zero.
- **We also swapped the loss.** The source uses mean squared error with a sigmoid classifier; we use `binary_crossentropy`, the conventional choice, which recovers far better from confidently-wrong predictions.
- **One run proves nothing.** Seeds reduce the variance between runs; they do not eliminate it. Two configurations one percentage point apart are indistinguishable on a single run each.

## Where this goes next

- **Session 8 (hands-on II):** reload this exact data and workflow, then make the model *good*. Force an overfit and watch the train/validation curves split apart; fix it three ways; sweep the learning rate; and read a confusion matrix on a dataset where the errors actually matter.
- **Session 13:** aim today's baseline instinct at a vendor — the "99% accurate" test that is right about 14% of the time once you know the base rate.
- **Session 9:** scale this same mechanism up until "a neural network" becomes "the LLM you actually use."

> **If you remember one thing:** you built a model that scored 55% knowing nothing and 96% after a few seconds of training — and the interesting engineering was never in the five lines that made it, but in knowing what those two numbers actually mean.
