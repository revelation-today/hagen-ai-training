# Self-Check Quiz — Session 10

Nine questions. Answers with explanations at the bottom. Aim to answer from understanding, not recall — several questions are about *why*, and two have deliberately tempting wrong answers.

---

### 1. In the prompt-engineering cycle, why does the guidance insist on changing only **one** thing per iteration?

### 2. You write a schema with a required, non-nullable field `root_cause: string` for an incident-triage prompt, and enable constrained decoding. The incident thread never states a root cause. What happens, and what is the fix?

### 3. Which of these is *not* a good use of few-shot prompting, and why?
&nbsp;&nbsp;**A.** Teaching the model your internal S1–S4 severity ladder.
&nbsp;&nbsp;**B.** Pinning an unusual output format you find hard to describe in prose.
&nbsp;&nbsp;**C.** Getting the model to generate ten genuinely varied project name ideas.
&nbsp;&nbsp;**D.** Showing the model what "omit this commit" looks like.

### 4. A colleague adds "Let's think step by step" to a prompt that classifies tickets into four severities, running on a current reasoning model. Give two reasons this is a poor change.

### 5. What is the difference between *asking* a model for JSON and *guaranteeing* schema-valid JSON — and name one failure mode the guarantee does **not** remove.

### 6. Your system message begins: `You are a config reviewer. Today's date is 2026-07-19.` The prompt runs 40,000 times a day. Name the specific cost problem and the one-line fix.

### 7. A vendor demonstrates that their multi-agent architecture outperforms a single agent by 90% on their internal evaluation. What single question most efficiently tests that claim?

### 8. You have a prompt whose eval-set pass rate has not improved across four honest iterations. List three things to change that are **not** the prompt's wording.

### 9. Which is more useful as a new eval case, and why?
&nbsp;&nbsp;**A.** A clean, representative input you wrote yourself to illustrate the task.
&nbsp;&nbsp;**B.** The malformed, truncated input that produced an embarrassing output in production last month.

---
---

## Answer key

### 1. One change per iteration

Because with two changes and one measurement you cannot attribute the result. If the pass rate rises from 60% to 80% after you added exemplars *and* rewrote the output contract, you have learned that *something* helped — which is not knowledge you can reuse, generalise, or defend in review. Worse, one change may have helped and the other hurt, and you have now baked in a regression you will never find. This is ordinary experimental discipline; prompts are unusual only in that people abandon it so readily. Corollary: it also tells you which parts of a long prompt are load-bearing, so you can delete the parts that are not.

### 2. Required non-nullable field with no answer in the source

**The model fabricates a plausible root cause.** Constrained decoding guarantees the output is *schema-valid*, not that it is *true* — and you have written a schema whose only valid outputs contain a root-cause string. You have not merely permitted a hallucination, **you have required one.**

The fix is an explicit escape hatch: `"root_cause": {"type": ["string", "null"], "description": "The stated root cause, or null if the thread does not state one."}` plus a system-message instruction to use null rather than infer. Add an `evidence` array of verbatim quotes and a `confidence` enum to make the remaining cases checkable. The general rule from `content/06`: **a required field with no null branch is an instruction to fabricate.**

### 3. **C** — generating ten varied project names

Few-shot works by making your demonstrated pattern the statistically obvious continuation. That is exactly what you want for A, B and D, and exactly what you do not want when variety is the goal: three example names will collapse the output distribution toward those three, and you will get ten variations on a theme rather than ten ideas. For creative tasks, prefer zero-shot with a raised temperature and an explicit request for *n* distinct options — and if you do use exemplars, use them only when you deliberately want a house style.

### 4. "Think step by step" on a reasoning model, for 4-way classification

Two independent reasons:

1. **Wrong technique for the task.** Severity classification is a single pattern-match, not a multi-step inference. There is no chain to reason through, so reasoning adds latency and tokens for no accuracy gain — see the task table in `content/04`.
2. **Wrong mechanism for the era.** On a current reasoning model, thinking is exposed as a *parameter with a budget*, and the model already reasons before answering. The instruction is redundant, and on some models it produces visible verbose rambling in the answer channel on top of the internal reasoning — which inflates cost and breaks the two-line output contract you wanted.

Bonus credit for noting that the change was made without an eval set, so nobody can tell whether it helped.

### 5. Asking vs. guaranteeing

**Asking** ("respond in JSON") is instruction-following: the model complies most of the time, and fails in familiar ways — a `Here's the JSON:` preamble, a markdown fence, a trailing comma, an invented extra field. At 98% compliance and 10,000 calls a day that is 200 parse failures daily, so you need a retry-and-repair path.

**Guaranteeing** means constrained decoding: the schema is compiled into a grammar and the sampler is restricted at each token to choices that keep the output valid. Invalid JSON becomes unrepresentable rather than merely unlikely.

**Failure modes the guarantee does not remove:** (a) **truncation** — hitting `max_tokens` mid-object still yields incomplete output, so always check the stop reason; (b) **hallucinated content** — the values can be schema-valid and completely wrong; (c) the model declining or emptying the object if you gave it no legitimate way to say "unknown."

### 6. The date in the system message

**The problem:** the system message is the static, cacheable prefix. Embedding a value that changes daily — or worse, a timestamp that changes per call — invalidates the cache prefix, so every one of those 40,000 daily calls pays full input price for the entire system prompt instead of the cached rate. Given typical order-of-magnitude cache discounts, this single line can multiply your input bill.

**The fix:** move the date out of the system message and into the user message, alongside the variable data. General rule from `content/07`: **static content first, variable content last.** Cache hit rate is an engineering outcome, not luck.

### 7. Testing the multi-agent claim

**"Was the comparison made at equal total token budget?"**

It is the single highest-yield question because token spend is the dominant confound. In the best-documented public case, the same write-up that reported a large multi-agent win also reported that **token usage alone explained roughly 80% of the performance variance**, with the multi-agent system consuming about **15× the tokens** — and neutral follow-up work found the architectural advantage largely disappears when the thinking-token budget is held equal. So the honest reading is that a good part of what looks like coordination winning is more tokens winning.

Acceptable near-equivalents: "what was the cost per task?" or "same scaffold and same eval set?" Both get at the same thing: is this a claim about architecture, or a claim about spending?

### 8. Four iterations, no movement — change something other than the wording

Any three of:

- **Model.** It may be too small for the task, or a reasoning model may be genuinely required. Test, don't assume.
- **Task decomposition.** Split one hard call into two or three easy, separately-testable calls. This is often the single biggest gain available.
- **Information.** The model may lack facts it cannot infer from the prompt. That is a retrieval/grounding problem (Session 13), not a prompting one — no phrasing supplies missing knowledge.
- **Output format.** Switch to a schema; a large class of "wrong answer" turns out to be "right answer, wrong shape."
- **The eval set itself.** The assertions may be wrong, contradictory, or testing something the objective never actually required.
- **The objective.** It may not be achievable, or two of its rules may conflict.

Recognising this is a senior skill: knowing when the prompt is not the problem saves days of prompt-golf.

### 9. **B** — the real production failure

Invented clean inputs test the case you already know works. Real failures test the boundary where the prompt actually breaks, and they are the reason the failure happened in the first place, which means they have already cost you something — harvesting them is the only way to get that cost back. They also come with realistic messiness (truncation, typos, missing fields, unexpected encodings) that you would never think to invent.

The practical rule from `content/08`: **build the eval set from real failures, five to start, growing toward 20–50, with a fifth held out** so you can tell the difference between a prompt that improved and a prompt that overfitted to your test cases — the same overfitting discipline taught in Session 8, wearing different clothes.

The best version of B: log human edits of model drafts in production. Every time a person corrects the model before shipping, that diff is a perfectly-labelled failure case, free, that most teams throw away.
