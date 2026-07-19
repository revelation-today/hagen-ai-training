# Lab — Session 6

**No hands-on lab — this is a concept session.** The hands-on build lands next session: in **Session 7** you will construct and train this exact 3→3→1 network in Keras (Colab-first; JupyterLite as the offline fallback) and watch its accuracy climb. Session 6 exists to make that lab feel earned rather than magical, so here we do a **reflective / diagram exercise** instead. It needs only pen and paper (or a whiteboard) and about 15–20 minutes.

---

## Reflective exercise: trace a forward pass by hand, then reason about training

### Part A — Draw the network (5 min)

From memory if you can, sketch the **3→3→1** network:

- 3 input slots labelled Red, Green, Blue.
- 3 hidden neurons, each connected to all three inputs.
- 1 output neuron connected to all three hidden neurons.
- Label the output "P(dark)".

Then **count the learnable numbers** (weights + biases). You should get **16**. If you don't, re-check: every arrow is one weight, every hidden and output neuron adds one bias.

### Part B — Push a new colour through (8 min)

Use the *same trained weights* from `content/03-forward-propagation.md`:

| | Weights (r, g, b) | Bias | Activation |
|---|---|---|---|
| Hidden $h_1$ | (0.5, −0.4, 0.2) | 0.1 | ReLU |
| Hidden $h_2$ | (−0.6, 0.3, 0.9) | −0.2 | ReLU |
| Hidden $h_3$ | (0.2, 0.2, −0.5) | 0.0 | ReLU |
| Output | (0.8, −0.5, 0.6) | 0.1 | sigmoid |

Take a **new** input colour: **near-black, RGB (26, 26, 26).**

1. **Scale** it by 255 → each channel ≈ **0.10** (so input ≈ (0.10, 0.10, 0.10)).
2. Compute each hidden neuron's weighted sum, then apply **ReLU** (negatives → 0).
3. Compute the output neuron's weighted sum over the three hidden outputs, then apply **sigmoid**. (You don't need a calculator to be exact — estimate: sigmoid of a small positive number is a bit above 0.5; of a negative number, a bit below.)
4. Apply the rule: **≥ 0.5 → DARK, < 0.5 → LIGHT.** What font does the network pick for a near-black background?

- **Self-check on the reasoning, not the decimals:** a near-black background should get **light** text to be readable. If your arithmetic lands the output *below* 0.5 (→ light), the network is behaving sensibly. Getting the *direction of the threshold* right (≥0.5 → dark) matters more than the third decimal place. Worked hint: the hidden sums are all small; $h_1 = 0.5(0.1) - 0.4(0.1) + 0.2(0.1) + 0.1 = 0.13$, $h_2 = -0.6(0.1)+0.3(0.1)+0.9(0.1)-0.2 = -0.14 \to \text{ReLU} \to 0$, $h_3 = 0.2(0.1)+0.2(0.1)-0.5(0.1)+0 = -0.01 \to 0$. Output sum $= 0.8(0.13) - 0.5(0) + 0.6(0) + 0.1 = 0.204$; sigmoid $\approx 0.55$. Hmm — that lands ≥0.5, i.e. **dark**, which for near-black is the *wrong* human answer. **That is the point of the exercise:** these weights were illustrative, not trained on real data, so they get an edge case wrong. Write one sentence on what that tells you (see Part C).

### Part C — Reason about training (5 min)

Answer these in a sentence or two each — no math:

1. The weights above got the near-black case wrong. In terms of **loss**, what happened, and what would **backpropagation** then do to the weights?
2. If you set the **learning rate** enormously high while training, what goes wrong? (Invoke the giant-vs-ant metaphor.)
3. You train until the model scores **99% on the training colours but 70% on colours it has never seen.** What is this called, and which number should you believe?

- **Answers to check yourself:**
  1. The prediction (≈0.55, "dark") was far from the truth (a near-black background should be "light" = 0), so this example's **loss is high**. Backprop pushes that error backward and hands each weight its share of the blame; gradient descent then nudges all 16 weights slightly in the direction that would lower the loss on this (and every other) example. Over many colours and many iterations, the weights stop making this mistake.
  2. Too-large learning rate = a **giant** taking strides so long it leaps over the bottom of the valley — training oscillates or diverges and never settles on good weights.
  3. **Overfitting.** Believe the **70%** (the held-out test score); the 99% is the memorised "answer key" and is not evidence the model works on new data.

---

## Optional take-home (2 minutes, before Session 7)

Watch **3Blue1Brown, *Neural Networks* chapter 1** (link in `resources/sources.md`). It animates exactly the neuron-and-layers picture you just drew. Watching it before the Session 7 lab will make the Keras code feel like something you already understand. (Link/pre-reading only — we do not reproduce its frames on any slide.)
