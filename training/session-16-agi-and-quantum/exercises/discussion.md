# Discussion & Polls — Session 16

For the 15-minute Q&A block plus two in-session polls. This is the **final** session, so the last two prompts are series-wide and deserve real time — do not let the quantum questions eat them.

---

## In-session polls (run these during delivery, not in Q&A)

### Poll 1 — the opening hook (Slide 3, ~0–3 min)

> **"Which of these is closest to what *you* mean by AGI?"**
> **A)** Can do any intellectual task a human can
> **B)** Is conscious / self-aware
> **C)** Can replace most jobs
> **D)** Can improve itself
> **E)** Passes as human in conversation

**Purpose.** The room will split, and the split is the lesson. Do **not** resolve it — say "hold that disagreement" and return to it on Slide 20.

**What a good discussion surfaces:** that these are five distinct claims with five distinct evidence requirements; that a system can satisfy A and E while failing B and D; and that "is AGI close?" is unanswerable until someone picks one. Watch for people who pick B (consciousness) — that is the branch most likely to consume the whole Q&A if unmanaged, and the session deliberately steers toward calibration as the testable proxy instead.

### Poll 2 — the upgrade poll (after Slide 11, ~27 min)

> **"Your team has a working AI-assisted workflow on model version N. The vendor releases N+1, described as more capable. Do you:"**
> **A)** Switch — it's strictly better
> **B)** Switch, then watch for problems
> **C)** Re-run your evaluation set on N+1 before switching
> **D)** Don't switch — it works

**Purpose.** Converts the "reasoning models hallucinate more" result into a decision the room actually owns.

**What a good discussion surfaces:** C is the answer, and the reason most teams pick A or B is that they have no evaluation set to re-run. That is the real finding. Push the follow-up: *does your team have 20 examples with known-good answers? If not, that's this month's task.* Frame it in their own vocabulary — this is a dependency upgrade without regression testing.

---

## Q&A prompts (the 15-minute block)

### 1. Which definition would you actually use?
> "If Qualcomm leadership asked you to assess whether 'AGI has arrived', which of the seven definitions would you pick — and what evidence would you go looking for?"

**What a good answer surfaces:** that the choice of definition determines the answer, so the definition must be chosen *before* the evidence is gathered, and openly. Strong answers pick a definition with a measurable test (Chollet's or ARC-AGI's) and reject the economic definition as unmeasurable from inside a company. Excellent answers note that the honest response to leadership is "the question as posed can't be answered — here's what I *can* tell you about capability on our specific tasks."

### 2. Which pillar matters most to *your* work?
> "Of the seven pillars, which single gap causes you the most trouble in practice — and what do you currently do about it?"

**What a good answer surfaces:** grounds the abstract framework in daily work. Expect **memory** (the model forgets context between sessions) and **learning** (it makes the same mistake every time and never improves) to dominate — both are correct, and both are structural rather than fixable by better prompting. This is a good moment to distinguish memory from learning again: a system with perfect memory and no learning repeats every mistake forever, with a perfect record of having made it.

### 3. Steelman the other side
> "Make the strongest possible case that AGI *is* close — using only evidence, not assertion."

**What a good answer surfaces:** intellectual honesty, and it protects the session from being one-sided. The genuinely strong points: the rate of benchmark saturation over five years; ARC-AGI-1 falling despite being designed to be memorisation-proof; capabilities like multi-step tool use that were absent three years ago; the fact that "this architecture can never do X" claims have a poor historical record. A facilitator who cannot make this case credibly has not earned the skeptical position.

### 4. Whose incentives are you trusting?
> "Every public AGI position is held by someone with a stake in it — including the skeptics, and including this training material. So how do you actually decide what to believe?"

**What a good answer surfaces:** the move from *"who do I trust?"* to *"what would falsify this?"* Good answers weight demonstrations over declarations, prefer independently replicated results, and look for the specific observation that would change the speaker's mind. The best answers apply it to this deck: *what would change the presenter's mind about AGI?* (Answer, if asked: sustained performance on held-out ARC-AGI-2-class tasks at reasonable cost, and any credible demonstration of post-deployment skill acquisition.)

### 5. Model upgrades as configuration changes
> "Should the model version behind an internal AI tool be a tracked configuration item, with a change process? What would that process look like?"

**What a good answer surfaces:** the most directly actionable idea in the session for this audience. Good answers name the pieces: version pinning, an evaluation set as an acceptance gate, a rollback path, and a defined owner. Expect pushback that "vendors deprecate old versions on their schedule" — which is true, real, and precisely why the evaluation set matters more here than for a normal dependency: you may not get to decline the upgrade.

### 6. Quantum — the honest version ⚠️
> "If someone in the business asks 'what's our quantum strategy for AI?', what do you say?"

**What a good answer surfaces:** the willingness to give a boring, correct answer. A good response separates the two questions: for AI, there is nothing actionable and no credible near-term path — say so plainly. For cryptography, there *is* an actionable programme, and it is crypto-agility, key and certificate inventory, and PQC migration planning for long-lived products. Watch for the trap of inventing a strategy to seem responsive.

### 7. Crypto-agility — the concrete one
> "What is the longest-lived product your area touches, and could its cryptography be replaced in the field?"

**What a good answer surfaces:** for a hardware-adjacent audience, this is the sharpest question in the quantum half. Many will not know the answer — which is itself the finding, and a good outcome. Follow up: *who would know, and is there a documented inventory of the keys and certificates involved?* "Harvest now, decrypt later" makes this present-tense, not future-tense.

### 8. 🏁 Series close — what changes on Monday?
> "Across all fifteen sessions: name **one** thing you will do differently, and one thing you now believe that you didn't in Session 1."

**What a good answer surfaces:** this is the closing prompt — protect at least four minutes for it and go around the room if the group is small enough. Good answers are specific and small ("I'll build an eval set for our triage prompt", "I'll stop pasting config data into a public tool", "I'll ask vendors for their base rate"). Vague answers ("be more careful") should be pushed once for specificity.

**Facilitator note:** end on this, not on quantum. The quantum segment is a horizon scan; the series is about judgement. Close where the value is.

---

## Questions you should expect, and honest answers

| Likely question | The honest answer |
|---|---|
| *"So when will AGI arrive?"* | Nobody knows, the labs disagree about whether the question is well-formed, and anyone giving you a date is expressing a belief. What we can say: current systems lack post-deployment learning and grounded world models, and there is no accepted plan for either. |
| *"Are LLMs conscious?"* | Unfalsifiable and not useful for any decision you will make. The testable proxy is calibration — does stated confidence match actual accuracy? Currently: imperfectly. |
| *"Won't scaling just solve all of this?"* | It has solved a lot, so the question is fair. But two of the five limits — reasoning depth and embodiment — show no sign of yielding to scale, and the labs are hedging by building different architectures. |
| *"Isn't this all just doom-mongering / hype-mongering?"* | Neither. The claim is narrow: these systems are useful and improving, **and** "general intelligence is imminent" isn't supported by the measurements. Both halves. |
| *"Should we invest in quantum now?"* | For AI: no, there's nothing to invest in. For cryptography: yes, and it's an engineering migration with published standards, not a research bet. |
| *"Will a quantum computer break our encryption?"* | Eventually, for public-key crypto (RSA/DH/ECC), on a contested timeline. Symmetric crypto is fine with longer keys. The urgency is "harvest now, decrypt later" — recorded traffic can be decrypted later. |
| *"You've been negative all series — do you actually use this stuff?"* | Yes, extensively, including in building this material. Skepticism is how you get value from a tool that fails silently. |
