# Discussion & Poll Prompts — Session 6

For the 15-minute Q&A. Each prompt lists what a good answer surfaces, so the facilitator can steer without lecturing. Two are live polls (quick hands / A-B-C); the rest are open discussion. Pick 3–5 to fit the time and the room.

---

## Poll 1 — "Is this problem a job for a neural network?" (A / B / C)

Read three scenarios; the room votes **A) yes, use a neural network / B) no, use a simpler model / C) it depends**, then discuss.

1. Predict next quarter's support-ticket volume from 12 months of tabular history.
2. Automatically tag screenshots of failing dashboards as "network," "auth," or "storage."
3. Flag which of last night's 4,000 config changes is most likely to have caused an incident.

- **What it surfaces:** the structured-vs-perceptual distinction from `content/02`. (1) is tabular → simpler model (regression) is usually better and cheaper. (2) is perceptual (images) → a neural network genuinely earns its place. (3) is tabular-ish → start simple; a network is likely overkill. The lesson: reach for the network only when no one can hand-write the rule.

## Poll 2 — "Which model would you ship?" (A / B)

Two models for the colour task:
- **Model A:** 99% accuracy on the training data, 71% on the held-out test data.
- **Model B:** 86% accuracy on training, 85% on test.

Vote A or B, then defend it.

- **What it surfaces:** the overfitting diagnostic from `content/06`. Model A memorised (huge train-test gap); Model B generalises. The higher training number is a trap. Almost everyone should land on B — and the discussion is *why the bigger number is the worse model.* Bridge to Session 13.

---

## Discussion 1 — Where would a held-out test set have caught a bad call?

Ask the room for a real example from their own work — a model, a benchmark, a dashboard, or even a vendor demo — where performance was measured on the same data it was built or tuned on.

- **What it surfaces:** that "evaluate on data the thing has never seen" is a general principle, not a neural-network detail. Release/problem/config folks will recognise it in capacity forecasts, anomaly baselines, and proof-of-concept demos. Good answers connect "training accuracy" to "the demo that looked great and then failed in production."

## Discussion 2 — The flashlight can't see the whole mountain. So how do we know it works?

Given that gradient descent only ever feels the slope right at its feet and can get stuck in a shallow valley, why do we trust trained networks at all?

- **What it surfaces:** an honest look at the limits from `content/05`. Good answers touch: we don't need the *global* best — a "good enough" valley that generalises is the goal; randomness (mini-batch) helps escape shallow valleys; and ultimately we trust it because the *test set* says it works, not because the optimisation is guaranteed. Reinforces that empirical validation, not theoretical guarantee, is what licenses a model.

## Discussion 3 — Why is the nonlinearity the whole game?

Push on it: if you removed every activation function, what exactly would a 50-layer network be able to do?

- **What it surfaces:** the collapse argument from `content/04`. A good answer: nothing more than a single linear model — 50 layers of linear maps compose to one linear map. The nonlinearity is what makes depth mean anything. This is the one "aha" that separates people who *get* neural networks from people who can only recite the diagram.

## Discussion 4 — Our example isn't even "deep." Does the label matter?

We admitted the 3→3→1 network has one hidden layer and so, by definition, is not deep learning.

- **What it surfaces:** healthy skepticism about marketing vocabulary (a course through-line). Good answers: the *mechanism* is identical whether it's 1 hidden layer or 100; "deep" is a threshold, not a different kind of thing; and vendors lean on the word "deep learning" for weight the specific architecture may not warrant. Ties back to Session 1's hype-deflation.

## Discussion 5 — Bridge to Session 7

"Next session you'll build and train this exact network in about five lines of Keras. What do you most want to *see* happen?"

- **What it surfaces:** primes the hands-on. Ideal answers: watch the untrained model score ~50%, then watch accuracy climb; watch the loss fall; try a bad learning rate on purpose. Sets expectations that the lab makes today's abstractions concrete.
