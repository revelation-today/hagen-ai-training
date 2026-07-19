# Discussion & Polls — Session 2

For the 15-minute Q&A block and the two in-session polls. Prompts are ordered so the first two are easy to answer and the later ones require the session's content. **You will not use all of these** — pick three or four and let them run.

---

## In-session polls (2 minutes total, run during the content)

### Poll A — after Slide 8 (the ¾-word rule)

> **"How many tokens is a 40-page specification?"**
> A) About 2,000 B) About 26,000 C) About 200,000

**Answer: B**, ~26,000 (≈20,000 words × 1.3). Most rooms guess A. That gap *is* the point: people systematically underestimate document token counts by an order of magnitude, which is exactly how "let's just attach the spec" becomes a 10× cost change nobody flagged.

### Poll B — before Slide 13 (the insight)

Show workloads X and Y — same 2,000 requests/month, one with a spec attached.

> **"How much more does Y cost than X?"**
> A) The same B) About 2× C) About 10× D) About 100×

**Answer: C**, ~10×. Take the show of hands *before* revealing. Whatever the room votes, ask the person who voted A to explain their reasoning — it will be a per-request model, stated out loud, which is the most useful thing that can happen in this session.

---

## Q&A prompts

### 1 — The seed question

> **"Where in your work would a per-request cost estimate have been wrong — and by how much?"**

*What a good answer surfaces:* participants applying the token model to something concrete from their own domain. Push for a number, not a feeling — "we'd have been out by about 5×" is worth more than "quite a lot." The best answers name a *specific context component* (a document, a history, a retrieval step). If the room hasn't measured anything, that itself is the finding: **the instrumentation gap is the most common state, and it's fixable in four lines of code.**

### 2 — The change-review question

> **"A developer opens a pull request that adds 'include the last 10 tickets from this component' to the prompt, to improve accuracy. It adds no new services, no new endpoints, and no new requests. Does it pass your change review?"**

*What a good answer surfaces:* the collision between existing change-control practice and token economics. It *would* pass most reviews today. It might multiply the bill by 5–10×. The productive follow-up: **what would have to be in a review checklist to catch it?** Realistic answers include a required token-delta estimate on any prompt change, a per-call token budget as an SLO, and treating retrieval `k` as a cost-bearing configuration item. This question is the one most directly aimed at this audience's actual job.

### 3 — The vendor question

> **"A vendor quotes you '€0.02 per request' for an AI-assisted triage tool. What do you ask them?"**

*What a good answer surfaces:* that the quote is denominated in the wrong unit, and that the risk is entirely on the buyer's side. Good questions from the room: What's the token budget per request, in and out? What do *you* put in the context that I can't see (system prompt, retrieved chunks, tool definitions)? Does it grow with conversation length? Is it an agent — how many model calls per user action? What happens to the price when I attach a document? Is there a cap? Who eats the overage?

Connect it forward: Session 13 is about evaluating vendor *accuracy* claims. This is the same skepticism aimed at the *pricing* claim, and it's the one you'll use first.

### 4 — The tier question

> **"Your team has a task running on a frontier model at €900/month. A colleague says a small model would do it for €15. What do you do before switching?"**

*What a good answer surfaces:* the discipline of measuring rather than assuming — the strongest answers describe an evaluation set of real inputs with graded outputs, run blind across both tiers, *before* the switch. Then the second-order point, which is the honest one: **a cheaper model that is wrong more often can cost far more in human rework than it saved in inference.** €885/month of savings evaporates against a few hours of an engineer's time spent correcting bad output. The saving is real; it is just not free, and it has to be measured on the output side too. Foreshadows Sessions 10 and 12.

### 5 — The counter-question (invite disagreement)

> **"Prices have fallen roughly an order of magnitude a year for equivalent capability. Doesn't that make all of this moot in eighteen months?"**

*What a good answer surfaces:* a genuinely open question, and the room should be allowed to argue it. The honest position has three parts: (a) unit prices really have fallen fast, and some of today's numbers will look absurd by 2027; (b) **consumption has grown faster than prices have fallen** — longer contexts, reasoning models that emit thousands of internal tokens, and agentic loops have all pushed tokens-per-task up sharply; (c) the *structure* survives regardless — input vs. output, quadratic conversations, caching prefixes, and "tokens not requests" are properties of how the technology works, not of this year's price list. Don't over-claim. Say which part you're confident about.

### 6 — The proportionality question

> **"We showed that at 2,000 tickets a month, even the most expensive tier costs about 1 % of the labour it displaces. So why spend a session on cost at all?"**

*What a good answer surfaces:* the room articulating the multipliers unprompted. Good answers name volume growth, context growth, agentic designs, and the fact that all three arrive *silently*. The best answers also make the honest counter-point: **at small scale, cost optimisation genuinely is the wrong first priority** — get it working and get it verified first. The session is about knowing which levers exist and which changes trip them, not about optimising a $17 line item.

### 7 — The vocabulary question (use if the room skews non-technical)

> **"Think of an automated system you already own — a router, a triage rule, a dashboard alert. Where does it sit on AI ⊃ ML ⊃ DL ⊃ LLM? And is that the right place for it?"**

*What a good answer surfaces:* that most existing automation is rule-based AI or nothing at all, and that this is frequently *correct*. Reinforce the discipline note: descend the stack only as far as the problem forces you. An auditable rule you can read, version, and diff beats an opaque model that is marginally more accurate — unless the margin is worth it. Watch for the reverse instinct too ("we should put an LLM on this"), and ask what it would buy that a rule can't.

### 8 — The data question (raise it if nobody else does)

> **"Everything we've discussed sends your text to somebody else's servers. What are you allowed to put in a context window?"**

*What a good answer surfaces:* that the cost model and the data-handling model are the same conversation — context *is* data egress. Don't attempt to resolve policy here; Session 14 covers it. But make the connection explicit, and reinforce the lab's rule: **no confidential ticket text, customer data, or internal source into a public tool.** If the room has an internal deployment, this is the moment to name it.

---

## If the room goes quiet

**Price their workload live.** Open the estimator from `exercises/lab.md`, ask for a real task, a realistic prompt size, and a monthly volume, and compute it on screen. Five minutes with the room's own numbers does more than any prepared prompt here — and it usually produces the objection you actually wanted ("wait, does that include the history?").
