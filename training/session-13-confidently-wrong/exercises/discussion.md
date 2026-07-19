# Discussion & Polls — Session 13

Material for the 15-minute Q&A block and for the two in-session polls. Prompts are ordered by how reliably they produce a useful conversation with this audience; run 3–4, not all eight.

---

## In-session polls (during the 45 minutes)

### Poll 1 — the hook (slide 3, minute 1)

> *"Our AI identifies **99%** of at-risk cases."*
>
> **A)** Buy it · **B)** Don't buy it · **C)** Need more information

**How to run it.** Show of hands, and **write the three counts on a flipchart where they stay visible.** Take one "C" answer and ask what they would want to know — accept it without evaluating it. Then move on without resolving anything. The visible flipchart is what makes the reveal land at minute 39.

**What it surfaces.** Most rooms split roughly between B and C, with a few A. The interesting group is C: they are already sceptical but usually cannot yet name *which* question would change the answer. That is precisely the gap the session fills — the difference between "I have a bad feeling" and "what was the base rate in your test sample?"

**The return.** After slide 17, re-poll with the same three options. The movement — and specifically people moving from C to B *with a reason* — is the payoff. Ask one person who moved to say what moved them.

---

### Poll 2 — is your human-in-the-loop real? (after slide 9, minute 15)

> Think of one AI-assisted step in your own workflow where a human reviews the output. Now answer honestly:
>
> **A)** The reviewer could have produced the correct answer themselves
> **B)** The reviewer can only judge whether it looks plausible
> **C)** Nobody actually reviews it any more

**What it surfaces.** C is common and rarely admitted out loud; making it an anonymous option (cards, hands-down-eyes-closed, or a poll tool) roughly doubles the honest C count. B is the dangerous middle — a control that exists on the org chart and cannot detect the failure mode it was created for, because plausibility is exactly where a good model never fails.

**Do not moralise about C.** C is usually the *correct* local response to a tool with 13.8% precision. Treat it as evidence about the tool, not about the reviewer — that framing is what keeps the room honest for the rest of the hour.

---

## Q&A prompts (the 15-minute block)

### 1. The seed question — start here

> **"Where in our own reporting do we quote a number that is technically true and practically misleading?"**

**What a good answer surfaces.** Every team has one, and it is almost never about AI. Candidates this room reaches for: change success rate (that counts changes nobody noticed), mean time to resolve (that excludes the tickets still open), test pass rate (on a suite that does not cover the risky paths), SLA compliance (measured on tickets that were correctly categorised in the first place). Getting the room to indict their *own* metrics before indicting a vendor's is what makes the session stick — it converts "vendors are shifty" into "measurement is hard and we do this too."

**Push if it stalls:** *"What is the denominator of our change success rate, and who chose it?"*

---

### 2. The base-rate audit

> **"Pick one AI tool we use or are being sold. What is the base rate of the positive class in our data — and could anyone in this room actually answer that today?"**

**What a good answer surfaces.** Usually: nobody knows, and nobody has been asked. That is the finding. The base rate is almost always computable from data the organisation already has (what fraction of commits produce an escaping defect? what fraction of incidents are genuinely P1 on review? what fraction of config changes are unauthorised?) — it just has never been anybody's job.

**The action item to aim for:** name one number, name one person, name one date. This is the prompt most likely to produce a real follow-up, and it is worth spending five of the fifteen minutes on.

---

### 3. The model upgrade

> **"Our vendor announces the model is now 99.5% accurate, up from 97%. What, if anything, should change in our process — and who is accountable for deciding that?"**

**What a good answer surfaces.** The reflexive answer is "nothing, that's good news." The session's answer: **a model upgrade invalidates the risk assessment.** The human control was calibrated to a 3% error rate; at 0.5% the reviewer will drift toward rubber-stamping, and the residual errors are now the subtlest ones. Somebody must re-derive the control.

Then the harder half of the question: *who?* In most organisations the answer is nobody — the upgrade arrives through a vendor release note, not a change record. For a configuration-management audience this is the moment the session becomes actionable: **the model version is a configuration item, and its accuracy is a property that other controls depend on.** If the room lands there on its own, that is the best outcome available in this session.

---

### 4. Sensitivity or precision — pick, and pay for it

> **"For one specific tool in our stack: would you rather it caught every real problem and buried you in false alarms, or flagged only real problems and silently missed some? Choose, and say what the choice costs."**

**What a good answer surfaces.** That there is no free option, and that the two failure modes have very different *visibility*. High sensitivity / low precision fails **loudly** and self-corrects socially — people stop reading the alerts, and you can observe that happening. High precision / low sensitivity fails **silently** and feels excellent, because every alert you see is real; you find out from field escapes months later. The second is usually the more dangerous configuration and the one people instinctively prefer.

**The follow-up that does the work:** *"If we pick high precision, what is our plan for measuring what we missed?"* Most teams have none, and recall is not measurable without deliberate effort — you have to go and find the negatives that were wrong.

---

### 5. Is p-hacking happening here?

> **"Have you ever kept iterating on an analysis until it showed what you expected — and stopped there? What would have caused you to stop earlier?"**

**What a good answer surfaces.** Everyone has, and the room needs one person to say so before it becomes discussable. The useful output is *not* confession; it is the second question. Answers that count: a metric written down before the analysis; a pre-agreed stopping rule; a colleague who did not want the result to be true; a held-back dataset touched once.

**Facilitation warning.** Hold the non-malicious framing hard. If this becomes an integrity conversation the room closes and you lose the last ten minutes. It is an *incentive design* conversation: what were we measured on, and what did that predictably produce?

---

### 6. The blast-radius question

> **"Name one place where an AI output triggers an automated action with no human gate. What is the blast radius if it's confidently wrong, and who finds out first?"**

**What a good answer surfaces.** Auto-closed tickets, auto-applied config, auto-generated release notes going straight to customers, agents with repository write access. The useful discrimination is between recoverable and unrecoverable: a wrong release note is embarrassing and fixable; a wrong config change on a production fleet is not. Not everything needs a gate — but the *decision* about which needs one should have been made deliberately, and usually it was never made at all.

**Watch for:** "the pipeline would catch it." Ask what the pipeline checks. Usually syntax, not intent.

---

### 7. Steelman the vendor

> **"Argue the vendor's side. Is there a version of this product we should buy?"**

**What a good answer surfaces.** Yes, and finding it is the constructive half of the session — the room should not leave believing all vendors are frauds. Three legitimate rescues: (a) **narrow the deployment population** so the base rate is genuinely high, e.g. only pre-screened commits from high-risk modules — changing the population is far cheaper than changing the model; (b) **stage it** — cheap high-sensitivity screen followed by a funded, mandatory, high-specificity confirmation; (c) **reduce what a positive triggers** — at 13.8% precision, "add a comment on the PR" is fine and "block the merge" is not.

This prompt is worth running whenever the room has become too pleased with its own scepticism.

---

### 8. The uncomfortable one

> **"If our own team published an accuracy number for something we built, would it survive the four questions?"**

**What a good answer surfaces.** Symmetry. Everything in this session applies to internal dashboards, internal tooling, and internal reporting, and the incentive pressures from `content/06` are stronger internally than externally, because nobody audits you. Run this one last, and only if the room has energy — it is the right place to end, but it needs a room that is not already defensive.

---

## Facilitation notes

- **Do not resolve poll 1 early.** The whole design depends on carrying an unresolved claim for 28 minutes.
- **The best material comes from the room, not the deck.** Prompts 1, 2 and 6 reliably produce examples better than anything scripted. Budget silence — this audience thinks before speaking.
- **Keep p-hacking non-accusatory.** Incentive design, not integrity.
- **Aim for exactly one written action item**, from prompt 2 or 3: a base rate somebody will go and compute, or a decision about who owns model-version changes. A session that produces one number and one owner has done more than a session that produced agreement.
- **If the room is quiet**, hand out the four-question card and ask people to apply it, in pairs, to a real tool they use. Five minutes of pairs, then two reports back.
