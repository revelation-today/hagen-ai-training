# Discussion & Poll Prompts — Session 3

For the 15-minute Q&A block (45–60 min) and the in-session polls. Each prompt states what a good answer *surfaces*, so the facilitator can steer toward the session's ideas rather than just collecting opinions. The room is release / problem / configuration managers and developers — keep every example in their world: change requests, incident records, build metrics, bug screenshots, support tickets.

The session's one keeper is the decision heuristic (`content/05`). If the Q&A drifts, steer back to two questions: **"what did you hold back?"** and **"does this problem actually need a neural network?"**

---

## Live polls (run these *inside* the 45 minutes, not in Q&A)

Show of hands or clicker. Each is placed against the slide it reinforces.

**Poll 1 — after the split section (slide 11).**
> A team reports: *"Our model is 99% accurate."* What is your **first** question?
> **A)** 99% of what — accuracy on which data?  **B)** How big is the model?  **C)** Which algorithm did you use?  **D)** How long did it take to train?

- *Target:* **A.** The number is meaningless until you know whether it came from training data or held-out data.
- *Surfaces:* whether "a model's accuracy on its own training data is not evidence" has landed. If the room splits toward C, the memorised-exam analogy needs one more pass before you move on.
- *Facilitator move:* follow up aloud — "and the second question is: how many times did you look at the test set?"

**Poll 2 — after the threshold section (slide 15).**
> A classifier flags configuration changes as risky. Its output crosses 0.5 and the change is blocked. Where did the **0.5** come from?
> **A)** It's the mathematically correct cut-off.  **B)** The model computed it during training.  **C)** It's a default someone probably never revisited.

- *Target:* **C.** 0.5 is a convention, not a result. The model outputs a probability; a human chooses where to cut.
- *Surfaces:* the false-positive / false-negative trade-off as a *business* decision. Expect someone to argue for B — that is the misconception worth correcting on the spot.

**Poll 3 — the decision poll, closing the session (slide 17, 37–45 min).**
> Read out three problems; the room votes **simple model** or **neural network** for each.
> 1. Predict next quarter's incident volume from historical monthly counts and the release calendar.
> 2. Classify a screenshot attached to a bug report as "UI glitch" vs. "crash dialog".
> 3. Flag a configuration change as risky from fields like files-changed, component, author-tenure, time-of-day.

- *Target:* **simple / neural / simple.** Data shape decides: tabular → simple; raw pixels → neural.
- *Surfaces:* whether the structured-vs-perceptual cut is now automatic. Any hesitation on #3 is the useful one — it *sounds* important and complicated, which is exactly the pull toward over-engineering the heuristic exists to resist.
- *Facilitator move:* on #3, add the auditability point — a decision tree can be printed and defended to a change board; a neural network cannot.

---

## Discussion prompts (pick 3–5 as time allows)

| # | Prompt | What a good answer surfaces |
|---|---|---|
| 1 | Pick a real prediction problem from your own team. What exactly are the **features**, what is the **label**, and **who or what produced the labels**? | The features/labels vocabulary made concrete, plus the labels-cap-the-model point from `content/01`. The best answers stall on "who labelled it" — that stall *is* the lesson. Historical tickets labelled inconsistently by twelve different people is a real, common answer. |
| 2 | Where in your organisation might a system already be **graded on its own homework** — a number reported from the same data it was built on? | Overfitting outside of ML: dashboards tuned until last quarter looks good, alert thresholds fitted to historical incidents, a "95% detection rate" measured on the incidents used to write the rules. The discipline generalises well beyond models. |
| 3 | The test set is meant to be touched **once**. In a real project with deadlines and stakeholders, what pressure makes people touch it more? What process would stop them? | The golden rule as a *process* problem, not a technical one. Good answers reach for CM instincts: seal the test set as a controlled artefact, separate the person who evaluates from the person who tunes, log every evaluation run. |
| 4 | Your model flags risky changes. Would you rather ship it with the threshold at **0.2** or **0.8** — and who in your organisation should actually make that call? | The false-negative / false-positive trade-off and its ownership. There is no correct number; a good answer costs both errors out loud (a missed bad change vs. ten wasted review minutes) and names a human owner — a change board, a service owner — not the data scientist. |
| 5 | A vendor pitches a deep-learning solution for a problem whose input is a table of twenty numeric fields. What do you ask them? | The heuristic used as a challenge tool. Good answers: what does a logistic regression or a decision tree score on the same data? Can you explain any single prediction to an auditor? What are the retraining and inference costs? |
| 6 | Training is expensive and one-time; inference is cheap and constant. Which of the two would show up in **your** budget, and what does that imply for a system called a million times a day? | The cost asymmetry from `content/02`. At scale, cheap-per-call still dominates total cost, and retraining is a recurring bill — a deployed model is a maintained dependency to re-qualify, not a shipped artefact. |
| 7 | The model was trained on last year's incidents. This year the toolchain changed. What happens, and what would tell you it happened? | Data drift arriving early (proper treatment in Session 13). Held-out testing is the *first* defence, not the last: it validates against yesterday's world. Good answers ask for ongoing monitoring of live accuracy, not a one-off test score. |
| 8 | Name a decision in your role where you would take an **auditable model that is 3 points less accurate** over a black box. Where would you *not*? | Balance — the course is skeptical, not anti-AI. Change approval, root-cause evidence, and anything an auditor will read want a readable model. Ranking search results or pre-sorting a screenshot queue genuinely do not. |

---

## Facilitation notes

- **Protect the honest voice.** Every "you can't trust that number" should be paired with what the number *would* have to be to be trustworthy. The goal is calibrated scepticism, not blanket doubt.
- **If someone raises the source-deck contradiction** (slide 16, the light/dark threshold flip), reward it — that is exactly the alertness the session is trying to build. Restate the general lesson: *a probability of 0.7 is useless until you know "0.7 of what?"*
- **Don't relitigate 70/15/15.** If a participant argues for cross-validation or a different ratio, agree immediately — the percentages are convention, the *principle* (evaluate on data the model never learned from) is what does not flex. `content/03` covers the exceptions: huge datasets, tiny datasets, time-series.
- **Park the deep-learning questions.** Questions of the form "but how does the network actually learn?" belong to Sessions 6–8. Note them visibly and move on; this session is about the machinery all methods share.
- **Land on the four questions.** The room should leave able to ask any team: *What are the features and the label? What did you hold back, and what did it score there? Where is the threshold and who chose it? Does this need a neural network at all?*
