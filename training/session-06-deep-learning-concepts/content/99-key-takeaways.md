# Key Takeaways — Deep Learning, Conceptually

A one-page recap of the whole session. If a colleague missed it, this is what they need.

## The machinery, bottom to top

| Level | What it is | The one-line version |
|---|---|---|
| **Neuron** | Weighted sum of inputs + bias, then an activation | A linear function wearing a nonlinearity. |
| **Layer** | Many neurons side by side | Each looks at the previous layer's outputs. |
| **Network** | Input layer → hidden layer(s) → output layer | Ours is 3→3→1: RGB in, one probability out. |
| **"Deep"** | More than one hidden layer | Ours has one hidden layer, so it's *not* technically deep — and that's fine. |
| **Forward propagation** | Push numbers through the fixed weights | Sum, activate, sum, activate → a prediction. |
| **Activation function** | The nonlinear bend between layers | Without it, any depth collapses into a single line. |
| **Loss** | One number for "how wrong" | Training's only goal is to shrink it. |
| **Gradient descent** | Downhill walk through the loss landscape | Flashlight in the mountains: feel the slope, step down, repeat. |
| **Backpropagation** | Send the error backward, assign each weight its blame | Forward makes the prediction; back assigns the blame. |
| **Overfitting** | Memorising instead of learning | Caught by a held-out test set. |

## The seven ideas to actually remember

1. **A neuron is a weighted sum plus a nonlinearity.** Weights and bias are the tunable parts; the activation is a fixed curve. Everything else is copies of this wired together.
2. **A network is layers of neurons; "deep" just means more than one hidden layer.** More layers = more capacity, but also slower, hungrier for data, and easier to overfit. Number of layers/neurons is chosen by experiment, not formula.
3. **Forward propagation is literally just arithmetic** — multiply-add, bend, multiply-add, bend — until one probability comes out. Our salmon-pink example gave 0.57. No magic step hides in the middle.
4. **The nonlinearity is the whole point.** Strip the activations out and a hundred-layer network can only draw a straight line. ReLU for hidden layers, sigmoid for a binary output, softmax for multi-class.
5. **Training starts random (chance accuracy) and improves by shrinking a loss.** Gradient descent is a flashlight-guided downhill walk; the learning rate is your step size (too big = a giant leaping over the valley; too small = an ant taking forever).
6. **Backpropagation distributes the blame backward** so even hidden weights know which way to move. Predict → measure error → assign blame → nudge weights, thousands of times.
7. **A low training score proves nothing on its own.** Overfitting means memorising; you only trust performance on a held-out test set. The best model generalises — it is not the one with the lowest training loss.

## Honest caveats we kept in view

- **We taught the ideas, not the derivations.** Gradient descent and backprop are shown by intuition — no calculus. The real derivations live in the source course and the 3Blue1Brown pre-reading.
- **Our example doesn't need a neural network.** Light-vs-dark is a tabular problem a logistic regression solves fine. We used a network purely because it is small enough to see through. Reach for the simpler model first in real work.
- **The threshold is ≥0.5 → DARK** (the output is the probability of *dark* text). The source deck contradicts itself on this; we resolved it in favour of DARK.
- **Classification normally uses cross-entropy loss, not the squared error** our example carries — a simplification the source made to keep one loss across the course. It changes nothing conceptual here.

## Where this goes next

- **Session 7 (hands-on):** build this exact 3→3→1 network in ~5 lines of Keras, run it untrained (watch it flail at chance accuracy), then train it and watch the accuracy climb. Everything mysterious this session becomes five lines of code.
- **Session 8 (hands-on):** make it *good* — see overfitting happen, then fight it with more data, early stopping, and tuning; read a confusion matrix.
- **Session 9:** scale this same mechanism up to a transformer and see how "a neural network" becomes "the LLM you actually use."

> **If you remember one thing:** a neural network is a stack of "weighted sum → bend" units whose knobs are learned by walking downhill on an error landscape in the dark — powerful, mechanical, and no more magical than that.
