# Key Takeaways — Learning From Data

A one-page recap of Session 3. If you read nothing else, read this.

## The through-line

Machine learning inverts traditional software. Instead of *rules + data → answers*, supervised learning is **data + answers → rules**: you supply worked examples and the machine infers the logic. Everything else in this session — and much of the Methods block — is a consequence of that flip.

## The five ideas

1. **Supervised learning learns from labelled examples.** **Features** are the inputs (the columns you feed in); the **label** is the answer (the column you want out). One row = one example. A model is only as good as its labels — always ask where the labels came from. Training is a **loop**: predict → measure error → adjust → repeat, until it stops improving.

2. **A model is a learned function** — a fixed structure plus settings (parameters) found by training — like the **dark-clouds-mean-rain** instinct: features in, prediction out, learned from experience, and capable of being *confidently wrong* or failing outside the conditions it learned in. **Training** is a one-time, expensive process over all the data; **inference** is a cheap per-call process over one input. At scale, inference cost dominates.

3. **Hold data back, always.** The training loop rewards *memorising*, so accuracy on training data is not evidence a model works. Split **70 / 15 / 15**: **train** fits the settings, **validation** tunes the choices, **test** is the one-time honest report card. The **test set is touched exactly once, at the very end** — any test result that influences a decision contaminates it. **Overfitting** (great on training data, poor on unseen data) is invisible without held-out data.

4. **Regression predicts a number; classification predicts a category.** A classifier is usually a regression underneath: it outputs a **probability**, then a **threshold** turns it into a decision. The default threshold (0.5) is a **business choice**, not a constant — you move it to trade false positives against false negatives, and only your organisation can say which error is worse.

5. **Use the simplest model that works.** **Structured/tabular data → simple models** (regression, trees, forests). **Perceptual/fuzzy data (images, audio, free text) → neural networks**, which earn their cost by learning their own features. Neural networks carry four standing costs — more data, more compute, opacity, harder maintenance — so reach for one only when a simpler model has genuinely failed. For accountability-driven roles, an auditable model often beats a marginally more accurate black box.

## The questions you can now ask any "we have a model" claim

- What are the **features** and the **label**?
- What data did you **hold back**, and what was the accuracy **on that held-back data** (not on training)?
- It outputs a probability — **where's the threshold, and who chose it**, weighing false positives against false negatives?
- Does this problem **actually need a neural network**, or would a simpler, auditable model do?

## If you remember one thing

> **Judge a model by its score on data it has never seen — and prefer the simplest model that hits the bar.** Training accuracy flatters; a neural network dazzles. Held-out performance and the simplest-thing-that-works are how you tell a real result from a demo that will break in production.

---

*Next: Session 4 takes the other branch — **unsupervised learning**, where there are no labels at all and the machine has to find structure on its own. Session 5 goes deep on the simple, auditable end of the flowchart (decision trees and random forests); Sessions 6–8 go deep on the neural-network end.*
