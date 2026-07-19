# Quiz — Session 7

Ten self-check questions. Answers and explanations at the bottom. Try them before scrolling.

## Questions

**1.** Our network is `Dense(3, activation="relu")` followed by `Dense(1, activation="sigmoid")`, with three inputs. How many learnable parameters does it have, and how do you get that number?

**2.** Why do we divide the RGB values by 255 before training? What is the specific reason we use `/255` here rather than fitting a `StandardScaler`?

**3.** You evaluate a freshly built, untrained model and get **0.55 accuracy** on a test set. Has it learned something? Explain what is actually happening.

**4.** Why does an untrained Keras network output roughly 0.5 for every input?

**5.** You train with `epochs=100` and `batch_size=32` on 896 training rows. How many times are the weights updated?

**6.** What is the difference between the `loss` and the `metrics` argument in `model.compile()`? Why can't we just optimise accuracy directly?

**7.** You call `model.fit(X_train, y_train, epochs=5)` on a model you already trained for 100 epochs, and the accuracy is excellent. What mistake have you made?

**8.** Your training log shows `loss: nan` from epoch 3 onward. What is almost certainly wrong, and what is the first thing you change?

**9.** A different log shows loss sitting at 0.693 and refusing to move, with accuracy stuck at exactly the majority-class fraction. What does that tell you?

**10.** Your model outputs `0.60` for mid-grey and `0.98` for cream. Both become "DARK text" after thresholding at 0.5. What information did you just discard, and what could you have done with it?

---

## Answer key

**1.** **16 parameters.** Input→hidden: 3 inputs × 3 neurons = **9 weights**, plus **3 biases** (one per hidden neuron) = 12. Hidden→output: 3 × 1 = **3 weights** plus **1 bias** = 4. Total 16. The general rule for a Dense layer is `(inputs × neurons) + neurons`. *(content/03)*

**2.** Because weights are initialised as small numbers near zero, and inputs of 0–255 are ~255× larger than the layer expects — early activations land deep in the sigmoid's flat region where gradients are tiny, so training is slower and less stable. We use `/255` specifically because **255 is a known, fixed maximum** determined by the colour format, so there is nothing to learn from the data. When the range is *not* known in advance, you fit a scaler **on the training set only** (fitting on all the data leaks test information into training). *(content/02)*

**3.** **No.** It has almost certainly predicted the **same class for every row**, and 0.55 is simply that class's share of the test set. A model that always answers "DARK" scores identically. The untrained accuracy is arithmetic about the dataset, with the model contributing nothing. This is why an accuracy figure alone can never distinguish "learned the problem" from "guessed the majority class." *(content/05)*

**4.** Keras initialises **biases at exactly zero** and weights as small values symmetric around zero (Glorot-uniform), so the output neuron's pre-activation value is close to 0 for any input — and `sigmoid(0) = 0.5`. The model has no reason to prefer either answer, so it doesn't. *(content/03, 05)*

**5.** **2,900 times.** `ceil(896 / 32) = 28–29` batches per epoch × 100 epochs. Weights update once **per batch**, not once per epoch — which is why halving `batch_size` doubles the amount of learning without changing `epochs` at all. *(content/06)*

**6.** **`loss` is what the model learns from; `metrics` is what gets reported to you** and never influences a single weight. We can't optimise accuracy directly because it is a **step function**: moving a prediction from 0.61 to 0.62 doesn't change it at all, and it jumps abruptly at 0.5. A function that's flat almost everywhere has no usable slope for gradient descent. So we minimise a smooth proxy (cross-entropy) and *watch* the thing we care about. *(content/04)*

**7.** Calling `fit()` again **continues training from the existing weights** — it does not start over. You just trained for 105 epochs, not 5, and your comparison is meaningless. **Rebuild and recompile the model before every comparison run** (this is what the lab's `build_and_compile()` helper is for). This catches almost everyone once. *(content/06)*

**8.** The **learning rate is too high** — each step overshoots the valley instead of descending into it, values diverge, and arithmetic produces `nan`. First move: **divide the learning rate by 10** (e.g. `Adam(learning_rate=1e-3)` instead of `1e-2`). `nan` loss is the classic signature; recognise it once and you diagnose it instantly forever. *(content/07)*

**9.** It is **not learning at all**. A loss of 0.693 is `ln 2` — exactly what you get from predicting 0.5 for everything on a binary problem — and the accuracy figure confirms it is just picking the majority class. Check three things in order: are the inputs scaled; are the labels really 0/1 and pointing the right way; does the loss function match the output activation (`sigmoid` + `binary_crossentropy`). This is a different failure from "learning slowly." *(content/04, 07)*

**10.** You discarded the model's **confidence**. 0.60 means "this is genuinely marginal" — and it is right, because mid-grey is ambiguous to a human eye too — while 0.98 means "certain". Collapsing both to the same label throws that away. You could **auto-decide the confident cases and route the uncertain ones to a human**, giving a system that is faster *and* safer than either full automation or full manual review. Worth also noting that the **0.5 threshold is a choice**, not a law; moving it is a deliberate trade between error types, which Session 8 makes explicit. *(content/07)*

---

**Score guide:** 8–10 you can build, train, and interrogate a Keras model unaided · 5–7 re-read `content/05` and `content/07` · below 5 re-run the lab, pausing at every checkpoint.
