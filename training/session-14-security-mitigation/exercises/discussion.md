# Discussion & Polls — Session 14

Six discussion prompts for the 15-minute Q&A block plus three in-session polls. The polls run *during* the 45 minutes and are timed to specific slides; the discussion prompts run after.

---

## In-session polls (during the 45 minutes)

### Poll 1 — after the Gandalf activity (≈ slide 4)

> **"What is the highest Gandalf level you reached?"** — 1 · 2 · 3 · 4 · 5 · 6 · 7+

*What it surfaces:* the distribution is the point. A room of non-specialists, with no tools and no preparation, will spread across levels 3–7 in under ten minutes. Show the hands, then say the sentence: *every one of those levels had a real, deliberately-engineered defence in front of it.* This is the empirical basis for everything in `content/02` §3.

*Facilitator warning:* do not let this become a competition about who is cleverest. Redirect immediately to "how long did that take us, collectively?"

---

### Poll 2 — before the data-leakage segment (≈ slide 12)

> **"In the last month, have you pasted work content into an AI tool that was not on an approved list?"** — Yes · No · I'm not sure what's on the approved list

*What it surfaces:* the third option almost always wins, and that is the finding. It reframes leakage from a discipline problem into a **communication and tooling** problem, which is the honest framing (`content/03` §5, `content/08` §3). Anonymous voting only — a show of hands here produces a false answer and damages trust for the rest of the session.

*If "I'm not sure" dominates:* stop and say so plainly. "That result means our policy is a communication failure, not a compliance failure. That's fixable, and it's what the closing exercise is about."

---

### Poll 3 — after the release-notes worked example (≈ slide 17)

Present the scenario: *an agent reads merged PRs and closed defects, drafts release notes, and publishes them to the customer portal on a schedule.*

> **"From a safety and security standpoint, is the spirit of this application: A) Safe · B) Unsafe · C) It depends"**

*What it surfaces:* the room will split, and the split is productive. Steer the debrief through the hazard triangle rather than to a verdict:
- Those voting **Safe** are usually thinking about the *drafting*, which is genuinely fine.
- Those voting **Unsafe** are thinking about the *publishing*, which is the initiating mechanism.
- **"It depends" is the correct answer, and the thing it depends on is exactly one design decision** — whether a qualified human gates the publish step. Same model, same data, same prompt; one architectural change moves it from unsafe to safe.

*Format credit:* the A/B/C case-poll format is lifted from the LLM-safety source deck, which uses it seven times. It works; reuse it.

---

## Q&A / discussion prompts (the 15-minute block)

### 1. The seed question

> **"Where in your area does an automated system already act on output that nobody reads?"**

*What a good answer surfaces:* not necessarily AI. Auto-generated tickets, auto-closed defects, scheduled reports that go to customers, config sync jobs, alerting rules that file changes. The realisation to reach for: **the human gate was already missing in places, and adding an LLM into that path is what makes it dangerous.** This connects the session to change control, which is the audience's actual expertise, and it usually produces the most concrete follow-up actions of the session.

---

### 2. The one that produces the best argument

> **"We've said prompt-level defences aren't real controls. So should we bother writing them at all?"**

*What a good answer surfaces:* the tension between "it reduces the success rate measurably" and "it creates false confidence." A good discussion lands on: **yes, write them — and never let anyone cite them as the reason a design is safe.** Watch for the failure mode where someone concludes "so nothing works" — push back: `content/01` §6b's output validation and `content/05`'s human gate are real controls; they are just not *prompt* controls.

---

### 3. The uncomfortable one

> **"Our sanctioned AI tool is slower and worse than the one people actually use. What happens?"**

*What a good answer surfaces:* leakage to unsanctioned tools is almost always a symptom of the approved path being inconvenient, not of people being reckless (`content/08` §3). The useful conclusion is that **tool provisioning is a security control** and belongs in the security budget. Expect someone to name a specific tool; do not let this turn into a complaint session — convert it into an action item with an owner.

---

### 4. The design question

> **"Take one AI use in your area. Which of the three legs — untrusted content, private data, outbound channel — is cheapest for you to remove?"**

*What a good answer surfaces:* applied use of the three-precondition test (`content/01` §4). Usually the answer is *private data* (run it with a narrower identity) or *outbound channel* (no egress, no auto-send). Listen for people identifying outbound channels they had not classified as such — rendered images, "share" buttons, tools that fetch URLs, tickets an outsider can read. That recognition is the highest-value moment in this prompt.

---

### 5. The one for the developers

> **"If 40–60% of generated security-relevant code carries a flaw, and we're shipping more code than ever — where does the review capacity come from?"**

*What a good answer surfaces:* that the productivity gain and the risk are the *same* phenomenon, and there is no version of this where review effort stays flat (`content/06` §4). Good answers reach for deterministic tooling (SAST/SCA in the pipeline — the highest-return control), treating generated code as third-party code, and being explicit about which code paths require expert review. Resist the framing "so we shouldn't use it"; nobody in the room believes that and it wastes the time.

---

### 6. The closing one

> **"What's the first thing you'll change on Monday?"**

*What a good answer surfaces:* commitment, specifically. Go around the room and take one sentence each. Good answers are small and checkable: *grep our system prompts for secrets; check whether our RAG retrieval enforces source permissions; find out who owns AI incident reporting; write the barred-uses rows for one deployment.* Vague answers ("be more careful") should be pushed once for specificity — that push is the most useful thing you do in the last five minutes.

---

## If the room goes quiet

Three restarts that reliably work:

| Prompt | Why it works |
|---|---|
| "Who here has a system prompt somewhere with something in it we'd rather not see published?" | Nobody answers, everyone thinks about it, and it usually produces an action item afterwards |
| "What's the most untrusted text that reaches a system you own?" | Concrete, factual, no confession required |
| "If the Gandalf password had never been in the context window, could you have got it?" | Rhetorical, and it re-lands the session's central point |

## If the room pushes back hard

Two objections you should expect, and the honest answers:

**"This is fearmongering — nothing bad has happened to us."**
Fair, and worth granting. The response is not more scare stories; it is that the controls being recommended (least privilege, output validation, a human gate before irreversible actions, an incident path) are the same controls this organisation already applies to non-AI systems. We are not asking for special treatment for AI. We are pointing out that AI tooling is currently being adopted *outside* the change-control discipline the room already owns.

**"The vendors will fix this."**
They will keep improving it, and the improvements are real. But the instruction/data boundary problem is architectural (`content/01` §1), and four years of insecure-code data (`content/06` §2) is direct evidence that scale alone does not close this class of gap. Plan for mitigation, and treat any vendor claim of a solution as a claim to be tested, not accepted — which is Session 13's lesson about vendor numbers, applied here.
