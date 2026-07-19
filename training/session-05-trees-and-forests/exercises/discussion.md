# Discussion & Poll Prompts — Session 5

For the 15-minute Q&A block and in-session polls. Each prompt lists what a good answer surfaces, so the facilitator can steer.

## Live polls (quick, hands-up or A/B/C)

**Poll 1 — before Slide 9.** *"A tree is deciding its first question. Which split would it prefer?"*
- A) One that makes both child groups a clean 50/50
- B) One that makes one child all-yes and the other all-no
- C) It doesn't matter; it picks randomly

**Answer: B.** All-yes / all-no is pure (Gini 0) — the least mixed, lowest cost. A surfaces the confusion that "balanced" sounds good; it's the opposite of what a tree wants. Use it to reinforce that low impurity = the goal.

**Poll 2 — before Slide 14.** *"Our forest scores 97% on test, our single tree 91%. Which do we ship for a decision that goes into a change-review record?"*
- A) The forest — 6 points more accurate
- B) The tree — we can read and defend every decision
- C) Depends on whether the decision must be justified

**Answer: C (leading to B for auditable decisions).** The point of the whole session: accuracy isn't the only axis. If each decision must be defensible, the readable tree may win despite lower accuracy.

## Discussion prompts

**1. Where in your work would an unreadable model be unacceptable?**
*Surfaces:* change approvals, incident root-cause, audit trails, regulated decisions — anywhere "because the model said so" won't survive review. Gets the room to own the interpretability argument rather than hear it.

**2. The tree learned that seniors with *excellent* credit didn't buy — a weird rule. Is that signal or noise, and how would you tell?**
*Surfaces:* tiny sample (only 14 rows), risk of overfitting to coincidence, the value of *seeing* the rule so a human can challenge it, and how a forest / more data would test whether it holds. Connects readability to oversight.

**3. A vendor pitches a random forest as "explainable AI." Fair or oversold?**
*Surfaces:* a forest gives feature *importances* (model-level), not a readable per-decision path — and MDI importances are biased toward high-cardinality features. "Explainable" is doing a lot of work; ask what exactly you can read. Rehearses vendor-claim skepticism (a Session 13 muscle).

**4. Out-of-bag error means you can skip a separate validation set. When would you *not* trust the OOB number?**
*Surfaces:* few trees → noisy OOB; data leakage or non-independent rows break the "unseen" assumption; for a number you'll report to management, still cut a real test set. Distinguishes a convenient estimate from a defensible one.

**5. When is a single tree the *wrong* choice even though it's readable?**
*Surfaces:* high-stakes accuracy with a human re-check anyway; genuinely perceptual/linguistic data (images, text) where trees underperform; unstable single-tree variance. Prevents "interpretable" from becoming a dogma — the honest counter-argument from `content/04`.

**6. Trees split on tabular features. What kinds of Qualcomm data are — and aren't — a natural fit?**
*Surfaces:* fit: incident attributes, config parameters, release metrics, categorical/numeric columns. Poor fit: raw logs as free text, images, waveforms — where you'd need features engineered first or a different model. Ties the method to their actual data.

**7. "An LLM can explain its answer too — just ask it why." What's wrong with treating that as the same thing?**
*Surfaces:* the LLM's explanation is *another generated output*, a plausible story that may not reflect the real computation; a tree's explanation *is* its mechanism. This is the sharpest single contrast in the session and worth ending on.
