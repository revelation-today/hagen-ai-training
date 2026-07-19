# Self-Check Quiz — Session 6

Eight questions. Answer from memory, then check the key at the bottom. If you can answer all eight, you're ready for the Session 7 lab.

---

**1.** What are the two steps a single neuron performs, in order?

**2.** In a neuron, which parts are *learned during training*, and which parts are fixed by the network designer?

**3.** Fill in the blank: a neural network qualifies as "deep learning" only if it has **more than one ______ layer.** Given that, is our 3→3→1 colour network deep learning? Why does the answer not really matter?

**4.** Our network outputs the number **0.57** for a colour. What does that number mean, and what text (light or dark) does the network recommend?

**5.** Why can't a network of purely linear neurons — no activation functions — do anything more than a single linear model can?

**6.** In the flashlight-in-the-mountains metaphor for gradient descent: what does the *flashlight* represent, and what does the *step size* represent? What happens if your steps are far too big?

**7.** In one sentence each: what does **forward propagation** do, and what does **backpropagation** do?

**8.** A model scores **98% accuracy on its training data but 74% on data it has never seen.** What is this called, how would you have detected it, and which number tells you the truth about real-world performance?

---
---

## Answer key

**1.** (i) A **weighted sum** of its inputs plus a bias (the linear step); then (ii) an **activation function** — a fixed nonlinear curve — applied to that sum (the nonlinear step). Sum, then bend. *(see `content/01`)*

**2.** **Learned:** the **weights and biases** (our toy network has 16 of them). **Fixed by the designer:** the architecture (how many layers/neurons) and the **choice of activation function**. Training only turns the weight-and-bias knobs. *(see `content/01`, `content/02`)*

**3.** More than one **hidden** layer. Our network has exactly **one** hidden layer, so **strictly it is not deep learning** — it's a (shallow) neural network. It doesn't matter because the **mechanism is identical**; "deep" is just a threshold on the number of hidden layers, and everything we learned transfers directly to deeper networks. *(see `content/02`; source error #13)*

**4.** 0.57 is the network's estimated **probability that the text should be DARK.** Since **0.57 ≥ 0.5**, the network recommends **dark text.** (Note: the source deck contradicts itself on this threshold; we hold ≥0.5 → DARK because the output is defined as the probability of *dark*.) *(see `content/03`; source error #1)*

**5.** Because a **linear function of a linear function is still just a linear function** — stacking linear layers collapses algebraically into a single linear layer. So without a nonlinearity between layers, a 100-layer network can only ever represent a straight line/flat plane, exactly like one linear regression. The activation is what lets depth express curves and complex boundaries. *(see `content/04`)*

**6.** The **flashlight = the local slope of the loss** (the gradient) — the only thing you can "see" at your current weights. The **step size = the learning rate.** Steps far too big = a **giant** that leaps over the bottom of the valley, so training oscillates or diverges and never settles. (Too small = an ant: correct but painfully slow.) *(see `content/05`)*

**7.** **Forward propagation:** pushes an input through the fixed weights, layer by layer (sum, activate, sum, activate), to produce a prediction. **Backpropagation:** takes the output error and pushes it *backward* through the network, giving each weight its share of the blame so gradient descent knows which way to nudge it. *(see `content/03`, `content/05`)*

**8.** **Overfitting** (the model memorised its training data rather than learning the general rule). You'd **detect it by evaluating on a held-out test set** the model never trained on and seeing the large train-vs-test gap. The **74%** (held-out/test score) tells the truth about real-world performance; the 98% is the memorised "answer key." *(see `content/06`)*

---

**Score guide:** 7–8 solid — ready for the Keras lab. 4–6 — re-read `content/05` (training) and `content/06` (overfitting); those carry the session. 0–3 — start again at `content/00-overview.md`; the pieces build on each other.
