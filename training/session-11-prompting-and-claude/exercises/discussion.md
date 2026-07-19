# Discussion & Polls — Session 11

Prompts for the 15-minute Q&A block, plus two in-session polls. Each carries a note on what a good answer surfaces, so the facilitator can steer rather than just collect.

---

## In-session polls

### Poll 1 — after slide 3 (the hook), 60 seconds

> Looking at the two release-note outputs: which defect is worst?
> **A)** It invented a version number **B)** It described an internal refactor as a customer feature **C)** It silently dropped two changes

**Most rooms pick A or B.** The answer worth arguing for is **C** — and the reason is the point of the session. A and B are visible: anyone proofreading catches them. C leaves no trace in the output. You cannot detect a silent omission by reading, only by comparing against the source, which is exactly what nobody does when the document reads well.

Use it to introduce the distinction that runs through the whole session: **errors you can see versus errors you can only find**, and the fact that the second category is what design has to defend against.

### Poll 2 — after slide 15 (the comparison table), 60 seconds

> The cheap model on the good prompt scored 16/20. The expensive model on the old prompt scored 14/20. Which surprises you more?
> **A)** That the cheap model won **B)** That the difference is only two cases **C)** Nothing — this is what I'd expect

Whatever the split, the follow-up is the same: *how would you have known this without running the suite?* The honest answer in every room is "we wouldn't — we'd have upgraded the model." That is a budget decision made on vibes, and it is being made in real organisations continuously.

---

## Discussion prompts

Pick three or four; do not attempt all eight.

### 1. Which of your recurring tasks would survive a 20-case test set — and which would fail it today?

**What it surfaces:** the gap between "this works" and "this is known to work". People discover mid-sentence that their most-trusted prompt has never been checked on an unusual input. Push for specificity: *what would case 1 be? What would the assertion be?* If someone can name the case and the assertion, they can build the suite this week, and that is the outcome you want from this session.

**Watch for:** the objection that their task is "too subjective to test". Usually false — the assertion is not "is this good" but "does it contain the ticket IDs", "does it avoid inventing a severity", "is the omitted list present". Push them toward properties.

### 2. Where in your workflow would a silent omission do the most damage?

**What it surfaces:** the highest-value question for this audience, and the one with the most role-specific answers. Release management: a missing breaking-change note that reaches customers. Problem management: an incident summary that omits the one contributing factor nobody wanted to write down. Configuration management: a review that covers three of four criteria and does not say which one it skipped.

**Where to steer it:** toward the design fix rather than the worry. Requiring an explicit "excluded, and why" section converts an invisible failure into a visible one. That is a five-word change to a prompt and it addresses the room's biggest stated risk.

### 3. Name one task you would *not* give to Claude, and say why.

**What it surfaces:** calibration, and it is a good opener because everyone can answer it. Good answers cite verifiability ("I couldn't check the result and the result matters"), determinism ("this is arithmetic, write the script"), or data handling ("that data doesn't leave our environment").

**Watch for:** two failure modes. Blanket refusal ("I wouldn't trust it with anything real") — probe for what would have to change, and you usually find the answer is "a way to verify", which is the session's point. And the opposite, a room where nobody can name anything — that is over-trust, and worth naming as such.

### 4. Someone shares a prompt in a channel and says it works well. What do you do next?

**What it surfaces:** whether the session's thesis actually landed. The target answer is "run it against the suite" — or, if there is no suite, "on what? how do you know?" asked without hostility.

**The productive tangent:** who owns a shared prompt? Copy-and-modify is how prompts spread and it is exactly how the drift problem starts. A prompt in a repo with a version and an owner behaves like code. A prompt in a channel behaves like a rumour. This often becomes the most practically useful five minutes of the Q&A.

### 5. What in your Project context, or your team's shared documents, is out of date right now?

**What it surfaces:** the context-rot problem, made personal. Nearly always someone realises aloud that a template or convention document changed months ago and the copy people work from did not.

**Where to steer it:** to a concrete commitment. Who owns which document, what "last verified" date goes in it, what the review cadence is. This is unglamorous and it is the highest-return action most attendees can take.

### 6. We said "most connector ideas should be a paste." Where is the line for you?

**What it surfaces:** whether the cost of a connector is understood as operational rather than technical. Good answers reason about frequency, volume, volatility, and how many people do the lookup. Weak answers reason about how interesting it would be to build.

**The follow-up that matters:** *who operates it, who reviews access, and what happens when it breaks at 2 a.m.?* If nobody can answer, the answer is "not yet". Then forward-reference Session 14 for the write-capable case — do not attempt the security discussion here, you will not finish it.

### 7. When has fluent output made you less careful?

**What it surfaces:** the honest version of the verification habit, and it works best if the facilitator answers first with a real example. Well-formatted output actively suppresses scrutiny — that is its most dangerous property, and it is a cognitive fact rather than a moral failing.

**Connect forward:** Session 13 formalises this — the better the system gets, the harder its rare errors are to catch, which inverts the intuition that higher accuracy is straightforwardly safer.

### 8. If the model version changed tomorrow, how would you find out something broke?

**What it surfaces:** usually silence, then "a customer would tell us." That is the honest answer for most teams and it is worth sitting with for a moment rather than rescuing.

**The point:** this is the single scenario the test suite is best at, and it is not hypothetical — model versions change on a vendor's schedule, not yours. Ten minutes of re-running a suite against an afternoon of building it, versus finding out downstream. If the room takes one action from this session, make it this one.

---

## Facilitation notes

- **Open with prompt 3.** Low barrier, everyone has an answer, and it establishes the skeptical tone the series carries.
- **Protect prompts 1 and 2.** They are the ones that produce actions rather than opinions.
- **Do not let the MCP discussion consume the block.** It is interesting and it is not what this session is for; forward it to Session 14 and to the reading.
- **Expect the "isn't this over-engineering?" challenge**, usually around test sets. Answer it directly and concede its true part: for a task done twice a month by one person who reads every output, yes, a suite is over-engineering, and the session says so. The claim is narrower — a prompt whose output goes out unreviewed, or runs hundreds of times, or feeds a system, is production software and deserves the same treatment as any other. Ask which of their tasks are in that category. Usually at least one is.
- **If the room is quiet on Part B**, ask directly: *"who has re-pasted the same background more than five times this month?"* Hands go up, and the conversation starts itself.
