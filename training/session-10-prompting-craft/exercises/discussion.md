# Discussion & Polls — Session 10

For the 15-minute Q&A block and the in-session poll. Six prompts, ordered so that a room that only gets through three still gets the valuable ones. Each has a note on what a good answer surfaces.

---

## In-session poll (run at slide 22, ~60 seconds)

> **Where is your team today?**
> **A.** Our prompts live in chat histories and people's heads.
> **B.** Our prompts are written down somewhere shared (a doc, a wiki page).
> **C.** Our prompts are in version control and have tests.

**What it surfaces.** Almost every hand goes to A or B, which is the point — nobody needs to feel behind, and it makes "level 3 is one afternoon away" land as an invitation rather than a rebuke. If a C hand goes up, spend two minutes on them: ask what their eval set looks like and how they found out it was needed. A peer's answer carries further than the slide.

**Facilitator note:** do not let this become a shaming exercise. The honest framing is that this discipline is about three years old and most of the industry is at A.

---

## Discussion prompts

### 1. Which of your recurring tasks is narrow enough for the cheap-model argument?

*Ask each role to name one: release manager, problem manager, configuration manager, developer.*

**What a good answer surfaces.** The qualifiers in the claim. The argument only holds for **narrow, well-specified, repetitive** tasks. Good candidates: severity classification, commit-log-to-release-notes, config-diff triage, log-error clustering, duplicate-ticket detection. Bad candidates: "decide whether to ship", "write the postmortem narrative", anything where the difficulty is judgement rather than specification. If someone proposes a bad candidate, that is the productive moment — ask what "good" would mean for it, and watch the objective refuse to be written down. That refusal *is* the diagnostic.

### 2. What would break if a prompt in your pipeline silently regressed next Tuesday?

**What a good answer surfaces.** Blast radius, and the absence of detection. Most teams have no mechanism that would notice — the output would still be plausible, still well-formatted, just wrong. This is the human-factors trap in miniature: if the prompt is right 99% of the time, spotting the 1% gets *harder*, not easier. Push toward specifics: who would notice, how long would it take, and what is the first thing they would look at. That list is the logging spec from `content/08`.

Also worth surfacing: the regression might not be yours. A provider updating the model behind a stable name produces exactly the same symptom, which is the argument for scheduled eval runs.

### 3. Where in your work would you *not* accept a well-formatted answer without checking it?

**What a good answer surfaces.** The core skepticism of the whole course, applied to structured output. Schema-valid is not true; constrained decoding removes parse errors and nothing else. A JSON record with an `S1` severity and a confident timestamp *looks* authoritative in a way a hedging paragraph does not — which makes the failure harder to catch, not easier. Good answers identify the places where formatting confers unearned trust: anything that auto-routes, anything customer-visible, anything that feeds a metric someone reports upward.

Follow-up if the room is quiet: *"who here would auto-file a ticket on model output with no human in the loop, and what would have to be true first?"*

### 4. "Let's think step by step" — who has used it, and who has tested whether it helped?

**What a good answer surfaces.** Almost every hand for the first question, almost none for the second. That gap is the session in one gesture. The follow-up: how many other prompting habits are you carrying that you inherited from a blog post and never tested? Then the generalisation — a prompting curriculum written 30 months ago is missing half its vocabulary, so assume the same about today's, which is exactly why the *testing loop* is the durable skill and the phrases are not.

**Facilitator note:** be careful not to make anyone feel foolish. It was correct advice for 2023 models. The lesson is about expiry dates, not gullibility.

### 5. Someone shows you a benchmark result proving their approach is better. What do you ask?

**What a good answer surfaces.** The equal-token-budget question, and the habits around it. A good answer gets to: *at what cost per call? against what baseline, given the same token budget? same scaffold/harness? same eval set, or one built after the fact?* Bring in the documented case where an architectural win was reported alongside the same paper's finding that token usage alone explained around 80% of the performance variance, at roughly 15× the tokens.

The turn that makes this stick: **apply it to your own team's next A/B result before someone else does.** This is the most transferable thing in the session — it outlives every technique taught today.

### 6. What is the smallest eval set you could build for one real task, before lunch?

**What a good answer surfaces.** That the barrier is imaginary. Five cases: a normal one, an empty one, an enormous one, a malformed one, and the one that embarrassed you last quarter. Push people to name their *actual* fifth case out loud — the specific past failure. Teams that can name it will build the set; teams that cannot are usually not logging enough to know what went wrong, which is its own finding.

If time allows, ask what the *assertions* would be. Watch how many turn out to be simple string checks (`does not contain "JIRA-"`, `under 12 lines`, `parses as JSON`). That discovery — that most useful checks are dumb and deterministic — removes the last excuse.

---

## Two questions the room will ask, with prepared answers

**"Isn't all this overkill for using a chat window?"**
Yes, for a one-off. No, the moment the same prompt runs twice a week or feeds anything automated. The dividing line is repetition: a prompt you will run once needs no eval set; a prompt you will run a thousand times is production configuration. Most people's real risk is misclassifying the second kind as the first.

**"Will prompt engineering still be a thing in two years?"**
The phrasing tricks will not be — half of them have already expired, and this session showed you one of them dying. What has replaced them is bigger, not smaller: deciding what goes into the context window at each step, and testing whether it worked. Be wary of the "prompt engineering is dead" framing; it is wrong, and it is usually selling something. Prompting did not die, it got absorbed into a wider discipline. The loop survives; the vocabulary churns.
