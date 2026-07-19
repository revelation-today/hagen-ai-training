# Lab — Session 1

**No hands-on lab — this is a concept session.** There is no code to run and no notebook to set up. (The first real Python lab is Session 7, in Google Colab with a JupyterLite fallback.) In place of a lab, do the reflective exercise below. It takes about 20–25 minutes and can be done solo as self-study or in pairs during the live session.

---

## Reflective exercise — "Catch a confident completion"

**Goal:** practise the session's central habit of mind — telling a *checked* pattern-completion from one you're *trusting because it sounds right* — on real material from your own work.

### Part A — Find one (5 min)

Recall a time in the last month when **a system or a person gave you a confident answer that turned out to be wrong.** Any of these count:

- an AI assistant that produced a fluent, plausible, incorrect answer (a wrong command, an invented API, a misremembered fact, a made-up citation);
- a dashboard, report, or automated ticket that stated something with authority and was wrong;
- your own memory of a meeting, a decision, or a config value that you were *sure* about and got wrong;
- a snap judgement about a person, a team, or a vendor that the evidence later didn't support.

Write down the example in one or two sentences.

### Part B — Diagnose it (10 min)

Run your example through the file-03 mechanism. Answer each:

| Question | Your answer |
|---|---|
| What **gap** was being filled? (What did the system *not actually know*?) | |
| What **pattern** did it complete with? (What "usually" is true here?) | |
| Was there any **evidence check** before the answer was delivered — or did confidence just ride on how plausible it sounded? | |
| How did the error **present** — obviously broken, or plausibly right? | |
| Which of the three coats is it closest to — **hallucination**, **false memory**, or **prejudice**? (Some fit more than one.) | |

Then the key one:

> **What single check, inserted before you acted, would have caught it?**

### Part C — Generalise it (5–10 min)

Pick **one recurring task in your role** where an AI (or an existing automated system) produces confident output you currently tend to trust. Sketch a two-line "verification rule" for it in the form:

> *Before I act on this output, I will check `____` against `____`.*

Examples to calibrate against (write your own, don't copy these):
- *Before I paste an AI-drafted release note, I check every version number and ticket ID against the actual changelog.*
- *Before I accept an AI root-cause summary, I confirm the claimed error appears in the actual logs (guards against intrinsic hallucination — the summary contradicting its own source).*
- *Before I trust a screening/ranking score about a person, I ask what examples it learned from and whether they're representative (guards against laundered skew).*

### Debrief prompts (if run in pairs or as a group)

- Whose example was a **plausible** error rather than an obvious one? Those are the dangerous ones — why did it get trusted?
- Did anyone's "verification rule" turn out to be *impossible* or *as much work as doing the task yourself*? That's the verification paradox (Session 13) arriving early — a real and honest outcome, not a failure of the exercise.
- Did any example resist the "lying vs. reconstructing" split? Discuss why "lying" almost never fits a system with no notion of truth.

### What to take away

You should leave with **one written verification rule you'll actually use this week**, and the reflex to ask, of any confident output: *checked against evidence, or trusted because it sounds right?* That reflex is the whole session, made portable.
