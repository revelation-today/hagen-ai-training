# Discussion & Polls — Session 15

The most important exercise file in the series. This session's 15 minutes of Q&A is not an appendix; for many participants it is the reason they came. Prepare it properly.

---

## Facilitator ground rules — read before delivering

1. **Do not reassure.** The instinct in this room will be to soften. Resist it. Every softening costs credibility, and this audience has heard the soft version already. If you do not know, say you do not know.
2. **Answer the question that was asked.** "Will there be layoffs?" is not a question about AI capability, and answering it as though it were is evasion that everyone will recognise. The honest answer is: *I don't know, that is a management decision rather than a technology outcome, and here is what the analysis does and does not support.*
3. **Keep it at task level.** If the conversation drifts to individuals or to specific teams, bring it back to sub-tasks. That is the frame in which the discussion is productive rather than anxious.
4. **You are allowed to be uncomfortable.** A slightly uncomfortable, honest session is remembered well. A comfortable, evasive one is remembered badly and taints the sessions around it.
5. **Watch who is silent.** In a mixed-seniority room the most exposed people speak least. The self-audit in `lab.md` exists partly so they have a private route into the material.

---

## Live polls (run during the session)

### Poll A — after slide 3 (the hook)

> **Which is closer to your last twelve months?**
> **A)** AI has measurably changed how I work
> **B)** I've tried it; it hasn't changed my actual output
> **C)** It's changed what arrives on my desk from other people
> **D)** Nothing has changed

**What it surfaces:** option **C** is the one to draw out — it is the fourth bucket, arriving before the session has named it. If several hands go up for C, you have a live example to reference for the rest of the session. If the room splits between A and B, that split *is* the S-curve: the same technology, wildly different realised value depending on how patterned the work is.

### Poll B — after slide 13 (the turn)

> **Of your typical week, which is the biggest share?**
> **A)** Tasks a competent AI could largely do today
> **B)** Tasks where AI could draft and I decide
> **C)** Tasks needing judgement, authority, or negotiation — AI contributes nothing safe

**What it surfaces:** the honest distribution, and it is usually more A than people expect once they think in sub-tasks rather than job titles. Follow up with: *"For those who said A — is that the part of your job you'd miss?"* The answer is almost always no, which is the genuinely useful realisation and a much better route into optimism than any reassurance you could offer.

### Poll C — after slide 17 (developer)

> **In the last month, have you shipped code or configuration that you did not fully understand?**
> **A)** Yes · **B)** No · **C)** I'd rather not say in this room

**What it surfaces:** comprehension debt, made real. Include option C sincerely — it is the honest option and offering it produces more truthful hands on A. Do not moralise about the result. The point is that the risk is present and normal, not that anyone did something wrong.

---

## Discussion prompts

Six prompts, ordered by provocation. Take them in this order; do not lead with the safest one. **Realistically you will cover three or four in 15 minutes** — pick based on the room, but Prompt 1 should always be one of them because it is the question everyone actually has.

---

### Prompt 1 — The direct one

> **"Be honest: which of the four roles in this session is most exposed, and why?"**

**Why ask it:** it is the question in everyone's head. Asking it yourself converts an undercurrent into a discussion you can facilitate. Not asking it does not make it go away; it makes it happen afterwards in a corridor, without you.

**What a good answer surfaces:** that exposure is not evenly distributed *within* a role. A coordination-heavy release manager is more exposed than a judgement-heavy one; that variation is larger than the variation between roles. The useful conclusion is that the exposure question is answerable about a *person's task mix*, not about a job title.

**Watch for:** the room converging on "developers, because of code generation." Push back — developers have the highest automatable share *and* the sharpest rise in demand for the remaining skill. High automation of sub-tasks is not the same as high exposure of the role.

**If asked "so is anyone actually safe?":** No, and safety is the wrong target. Nobody is safe from *recomposition*; everybody's job changes. What varies is whether what remains is more valuable or less, and that depends on task mix, which you can influence.

---

### Prompt 2 — The one about management

> **"If AI-assisted output doubles and verification headcount doesn't move, what actually happens? Walk it forward six months."**

**Why ask it:** it moves the discussion from "will I be replaced" — unanswerable — to "what is the operational consequence of a decision my organisation is making right now," which is answerable, concrete, and squarely within this room's professional competence.

**What a good answer surfaces:** scrutiny per unit falls silently; nobody decides it; the first visible symptom is an incident; the incident is attributed to the reviewer rather than to the ratio. This is a systems-thinking answer and this audience is unusually good at producing it.

**Push toward:** *what measurement would let you see this coming?* Change volume per reviewer, review time per change, defect escape rate. That is a concrete, actionable output and it is the sort of thing this room can implement without permission.

---

### Prompt 3 — The uncomfortable one about juniors

> **"If AI does the tasks we used to give juniors, where does the next generation of senior judgement come from?"**

**Why ask it:** it is the hardest unsolved problem in the analysis, it affects everyone in the room whether or not they manage anyone, and it is the clearest possible demonstration that this session is not selling comfort.

**What a good answer surfaces:** that the automatable-task set and the apprenticeship-task set are nearly the same set. That the effect is delayed by several years, which means nobody feels urgency now and everybody feels it later. That "we'll just hire seniors" fails at the industry level even where it works for one company.

**Possible directions, offered as options rather than answers:** deliberately reserving some work for humans as training; apprenticeship on *review* rather than on production (reviewing AI output is a genuine teaching activity, and arguably a faster one); pairing juniors with seniors on the judgement work rather than the production work.

**Do not resolve this.** Say plainly that you do not have an answer and that nobody does. That admission buys more trust than anything else in the session.

---

### Prompt 4 — The one that flips the frame

> **"Forget defence. Where in your work could AI do something genuinely valuable that nobody has tried, because it wasn't worth a human's time before?"**

**Why ask it:** every prompt so far is defensive, and a session that is only defensive is incomplete and slightly dishonest — there is real upside here and it is being left on the table.

**What a good answer surfaces:** work that was previously uneconomic. Reading *every* ticket from last year rather than a sample. Summarising every post-mortem into a themes report. Checking every change description against a standard. Translating internal documentation into every language the team actually speaks. **The pattern: tasks where the value was real but the human cost made them not worth doing.** That is where the honest upside lives, and it is a much better answer than "it drafts my emails."

**Push toward:** one concrete thing each person could try within two weeks.

---

### Prompt 5 — The one about your own judgement

> **"Name one thing your review caught in the last quarter that no tool would have caught. Now: is that written down anywhere?"**

**Why ask it:** it converts the abstract claim "judgement stays human" into personal evidence, and then immediately exposes that the evidence is invisible to anyone making resourcing decisions.

**What a good answer surfaces:** everyone has an example, usually a good one, and almost nobody has recorded it. Judgement that is not visible gets optimised away — not maliciously, but because it does not appear in any system anyone reviews.

**The action:** keep a catch log for one quarter. Date, what was caught, what it would have cost. It takes two minutes a week and it is the single most effective defensive act available to anyone in this room. This is the most practical outcome of the whole session; make sure it lands before the time runs out.

---

### Prompt 6 — The one about the S-curve

> **"Do you believe the S-curve argument? It was made before the current scaling era."**

**Why ask it:** it invites the room to attack the session's central claim, which is the best possible demonstration that this is analysis rather than advocacy. It is also genuinely contestable.

**What a good answer surfaces:** that the argument was wrong about the ceiling's height and right about the cost's shape. That saturation moved up the difficulty scale rather than vanishing. That the three structural failures are not on the curve at all — no amount of spend buys a guarantee.

**If someone argues capability *is* effectively exponential:** take it seriously, then ask the operational question. *Grant it. What is your organisation's plan for verifying the output of a system that improves faster than your ability to check it?* That question is uncomfortable under either belief, which is exactly why it is the right one.

---

## Two questions you will probably be asked

**"Should I be worried about my job?"**
The honest answer, and say it in roughly these words: *Worried, no. Attentive, yes. The specific thing worth attention is not replacement — it is being handed the verification load without the time to do it, which is a real and near-term risk. And the specific defence is making your judgement visible, because the decision about whether to staff verification is made by people who can only see what is written down.*

**"Is the company planning layoffs because of AI?"**
Do not speculate, and do not deflect either. *I don't know, and I'm not the right person to ask — but I will say that nothing in this session's analysis implies it follows from the technology. Composition change is the well-supported claim; headcount is a management decision made on other grounds.* Then, if appropriate, offer to take the question to whoever can answer it. **Agree this answer with the training requester before delivery** (see README).

---

## After the session

Assign the self-audit in `lab.md`. It is 25 minutes, it is private, and it converts a discussion into an artefact — which for participants who did not speak is the main route into the material.
