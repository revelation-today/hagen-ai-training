# Self-Check Quiz — Session 15

Eight questions. Answers at the bottom. If you get fewer than six, re-read `content/02` and `content/05`.

---

**1.** What three conditions must hold for a task to be one an LLM does genuinely well?

**2.** A colleague says: *"LLMs can't reliably do arithmetic, but that's just a current limitation — the next generation will fix it."* Which of their two claims is right, and which is wrong? Explain the distinction you are using.

**3.** State the S-curve argument in one sentence. Then state the one-sentence honest caveat about whether it survived the LLM scaling era.

**4.** Your team is asked to certify that two deployed environments are identical. A colleague proposes having an LLM compare the two configuration dumps and report differences. What is wrong with this, and what should be done instead?

**5.** Name the three mechanisms that make work *harder* because of AI adoption — the fourth bucket.

**6.** A problem manager receives a well-written, confident AI-generated root-cause narrative early in an investigation. Name the specific cognitive risk, explain why a wrong root cause is worse than no root cause, and give the concrete countermeasure recommended in this session.

**7.** Which of these is *not* a reason the proof-of-concept-to-production gap fails to close?
   a) It is downstream of the S-curve.
   b) It grows with deployment rather than shrinking with capability.
   c) Accountability cannot be automated even in principle.
   d) Models are not yet large enough to handle production complexity.

**8.** Apply the three-question decision rule to this task: *"Draft the customer-facing summary of last week's outage."* Walk through all three questions and state who owns what.

---
---

## Answers

**1.** (a) The task is a **language → language transformation**; (b) the **information needed is already present in the input**; (c) a human can **verify the output cheaply**. All three are required. The third is the safety property and the one most often skipped — this is the same rule as "an LLM application is defensible when the user can easily verify the output, or when truth is irrelevant." (`content/01` §1)

**2.** The **first claim is right** (LLMs are unreliable at arithmetic — it is not a language transformation) and the **second is roughly right but for the wrong reason, and is the wrong way to think about it.** The distinction is *"can't yet"* (quantitative, will improve, plan for it) versus *"can't, structurally"* (follows from the mechanism). Arithmetic is in a third category: it is solved not by the model getting better at maths but by **giving the model a tool to call**. The right response is architectural — make the model call a calculator rather than be one — not waiting for a better model. (`content/02` §1; `content/01` §3)

**3.** *AI skill coverage follows a logistic curve that saturates well short of 100%, while the cost of each additional increment of coverage rises steeply — it is not AI capability that is exponential, it is the expense of producing it.* Caveat: **the argument was wrong about the height of the ceiling and right about the shape of the cost.** Capability rose much further than predicted, but last-mile economics behaved exactly as described, and the last mile is where this audience works. (`content/03`)

**4.** Certification of identity is a **guarantee** — the sentence contains "identical," which is in the always/never/all/exactly family. An LLM produces a *sample*, never a proof; it can be right many times and still miss a difference, with no confidence signal you can trust and no coverage metric. **Use a deterministic tool** — a diff, checksums, or a policy-as-code validator — to establish the facts, then optionally use the LLM to *summarise the checker's output in human terms*, which is a legitimate and valuable use. Facts from deterministic tools, readability from the LLM, truth and authority from the human. (`content/02` §3; `content/08` §2)

**5.** (a) **Everyone else's output is now AI-shaped** — longer, more fluent, more uniform, so fluency has been decoupled from competence and a career-long reviewer heuristic no longer fires. (b) **The verification paradox** — a system that is right 99% of the time makes the human reviewer worse, because humans are documented to be poor at catching infrequent automation errors. (c) **Volume rising without matching verification capacity** — a silent, arithmetic reduction in scrutiny per item that nobody decided. (`content/05` §3)

**6.** The risk is **anchoring**: once a plausible explanation is in hand, subsequent evidence is unconsciously fitted to it and disconfirming evidence is discounted. It is worse than before because a wrong hypothesis used to arrive with a colleague's uncertainty attached and now arrives as confident, well-organised prose with no uncertainty markers. A wrong root cause is worse than none because **the problem record is closed, corrective action is taken against the wrong thing, budget is spent — and the real cause is still live, now with a record claiming it was addressed.** Countermeasure: require **at least three competing hypotheses with the disconfirming evidence for each**, never a single narrative, and prompt for the strongest argument against the leading explanation. (`content/07` §3a)

**7.** **(d).** Model size is not the constraint. The gap is structural: downstream of the S-curve (a), growing with deployment surface rather than shrinking with capability (b), and containing accountability, which is a social and legal relationship rather than a capability and therefore cannot be automated even in principle (c). (`content/04` §4)

**8.** **Q1 — if it's wrong, who answers?** You do; it goes to a customer with the organisation's name on it. So the AI cannot own it. **Q2 — can you verify it cheaply?** Yes, provided you or a colleague actually lived the outage — you can read the draft against what you know. (If nobody in the loop knows the incident, the answer is no, and you must not use a generated draft.) **Q3 — does it need a guarantee?** The *narrative* does not, but any specific claim in it — affected customers, duration, data exposure — does, and those must come from deterministic sources rather than from the model. **Verdict:** AI drafts the narrative from a verified factual skeleton; you supply and check the facts from authoritative sources; you edit, you send, you own it. Bucket: **augmented**. (`content/10` §1)
