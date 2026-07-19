# Self-Check Quiz — Session 2

Nine questions. Answers with explanations at the bottom. Questions 5–9 need arithmetic; a calculator is fine. Use the illustrative tier prices from `content/04` §1:

| Tier | Input $/1M tokens | Output $/1M tokens |
|---|---|---|
| **A — Frontier** | $15.00 | $75.00 |
| **B — Workhorse** | $3.00 | $15.00 |
| **C — Small** | $0.25 | $1.25 |

---

### 1. Vocabulary

A ticket-routing system uses 340 hand-written keyword rules maintained by two engineers. Where does it sit in AI ⊃ ML ⊃ DL ⊃ LLM, and what is its characteristic failure mode?

### 2. Training vs. inference

Your manager says: "I read that training a frontier model costs hundreds of millions of dollars. We can't afford to use AI." What is wrong with the inference, and what is the correct thing to worry about instead?

### 3. Parameters

True or false, with a reason: *"Model X has 405 billion parameters and Model Y has 8 billion, so X will be more accurate on our ticket-classification task."*

### 4. Tokens

Rank these four inputs from **cheapest to most expensive** in tokens, for the same amount of information conveyed:
(a) an English prose paragraph (b) the same content as a JSON object (c) the same content in German (d) the same content as a markdown table

### 5. Estimation

A design document is 12 pages of English prose, roughly 500 words per page. Approximately how many tokens is it? What does it cost to send once on Tier B?

### 6. The output lever

A call uses 2,000 input tokens and 400 output tokens on Tier B. What percentage of the *cost* is output? You can cut 200 tokens from either the prompt or the response — which saves more, and by how much?

### 7. The insight

Two teams each run **exactly 5,000 requests per month** on Tier B. Team 1 sends 1,000 input / 200 output per request. Team 2 sends 1,000 input / 200 output **plus a 30,000-token reference manual** on every request. Compute both monthly bills. What is the ratio, and what does it tell you about request-count-based estimates?

### 8. The conversation curve

A chat uses a 400-token system prompt, 150-token user messages, and 350-token replies. How many **input** tokens are billed on **turn 4**? (Turn 1 sends the system prompt plus the first user message.)

### 9. Caching

A team caches a 20,000-token stable prefix and reports "our cache hit rate is 0 %." Their prompt template is, in order: `[current UTC timestamp] [system prompt] [product manual] [user question]`. Diagnose the problem and give the one-line fix. Roughly what would the fix save on Tier B, at 10,000 calls a month? (Cache read ≈ $0.30/1M tokens.)

---
---

## Answer key

### 1.
**Rule-based AI — inside the AI circle but outside machine learning.** A human wrote every rule; nothing was learned from data.

Its characteristic failure is **silent rot**: a component gets renamed, a log format changes, and a rule quietly stops matching. Contrast with an ML system, which fails by *drifting* — still producing confident answers, at a slowly worsening rate. Both are quiet failures, but they need different detection: rule coverage monitoring for one, output-quality monitoring for the other.

Bonus point if you noted that being rule-based is not a criticism. It is auditable, deterministic, versionable, and diffable — all things a release/configuration discipline values. (`content/01` §3)

### 2.
The inference confuses **training cost** with **inference cost**. Training that model already happened, at the vendor's expense. When you call a hosted LLM you pay **only for inference** — running your tokens through numbers that already exist — and that is fractions of a cent per call.

The correct thing to worry about is **token volume**: tokens per call × calls, where "tokens per call" includes the system prompt, the conversation history, attached documents, retrieved chunks, and tool results. A large bill comes from context growth and call multiplication, never from the vendor's training run. (`content/02` §2)

### 3.
**False**, or at best unsupported.

Parameter count tells you about **hardware requirements** and roughly explains the vendor's pricing tier. It does not tell you about task accuracy. Training-data quality, instruction tuning, and distillation mean smaller models routinely match much larger ones on well-scoped tasks — and ticket classification with few-shot examples is exactly such a task.

The right response is not an argument, it is an experiment: run 200 real tickets through both, grade the outputs blind, and compare the quality gap against the price gap. (`content/02` §3)

### 4.
**(d) markdown table → (a) English prose → (c) German → (b) JSON** — roughly.

- Markdown table and prose are both close to the ~1.3 tokens/word baseline; a compact table usually wins because it drops connective words.
- German runs ~1.8–2.2 tokens/word — long compounds fragment, and the vocabulary was fitted mostly on English.
- JSON runs ~2.0–3.0 — braces, quotes, colons, and *repeated key names on every record* all cost tokens.

The operational point: **serialisation format is a free cost lever.** Sending 50 records as a markdown table instead of JSON can halve the input cost for identical content, at zero quality cost. (`content/03` §3)

### 5.
12 pages × 500 words = **6,000 words**. At ~1.3 tokens/word, that is **≈ 7,800 tokens** (round to ~8,000).

Tier B input: 8,000 × $3 / 1,000,000 = **$0.024**, about two and a half cents.

Sounds trivial — and it is, *once*. Attach it to every call in a 10-turn conversation and it is re-sent 10 times. Attach it to 100,000 calls a month and it is **$2,400/month**. The unit price is never the story; the multiplier is. (`content/03` §3, `content/05` §3)

### 6.
- Input: 2,000 × $3/1M = **$0.006**
- Output: 400 × $15/1M = **$0.006**
- Total **$0.012** → output is **50 %** of the cost, from 16.7 % of the tokens.

Cutting **200 output tokens** saves 200 × $15/1M = **$0.003** (25 % of the call).
Cutting **200 input tokens** saves 200 × $3/1M = **$0.0006** (5 % of the call).

**The output cut saves 5× as much** — the exact ratio of the unit prices. Hence: "Be concise. Maximum 100 words." is a cost control. (`content/04` §2)

### 7.
**Team 1:** (1,000 × $3 + 200 × $15) / 1M = $0.003 + $0.003 = **$0.006/call** → × 5,000 = **$30.00/month**
**Team 2:** (31,000 × $3 + 200 × $15) / 1M = $0.093 + $0.003 = **$0.096/call** → × 5,000 = **$480.00/month**

**Ratio: 16×**, on identical request counts.

What it tells you: **a request-count-based estimate is measuring the wrong quantity, and it will be wrong in the expensive direction.** Contexts only ever grow — someone attaches a document, someone raises the retrieval `k`, someone adds history — and none of those changes show up as additional requests. Any cost forecast, vendor quote, or change review framed in requests is blind to the variable that actually drives the bill. (`content/04` §3)

### 8.
Build the history up:

| Turn | Sent as input | Running history *after* the reply |
|---|---|---|
| 1 | 400 + 150 = **550** | 550 + 350 = 900 |
| 2 | 900 + 150 = **1,050** | 1,050 + 350 = 1,400 |
| 3 | 1,400 + 150 = **1,550** | 1,550 + 350 = 1,900 |
| 4 | 1,900 + 150 = **2,050** | — |

**Turn 4 bills 2,050 input tokens** — for a user message the person typed 150 tokens into. Nearly **4× turn 1**, and the gap keeps widening: the general form is `550 + (n−1) × 500`, so cumulative cost grows quadratically with turns. (`content/05` §2)

### 9.
**Diagnosis:** prompt caching works on a **prefix**. The UTC timestamp is the *first* thing in the template and changes on every call, so the prefix is different every time and **nothing after it can ever be cached.** One variable token in the wrong position invalidated 20,000 cacheable tokens.

**Fix (one line):** move the timestamp to *after* the stable content — `[system prompt] [product manual] | [timestamp] [user question]`. Stable first, variable last. It is a reordering, not a rewrite, and it changes no behaviour.

**Saving:** 20,000 tokens/call at $3.00/1M = $0.060 currently; at the cache-read rate of $0.30/1M = $0.006. That is **$0.054 saved per call**, × 10,000 calls = **≈ $540/month**, about a **90 % reduction on that portion**.

Two honest caveats worth stating: you pay a **cache-write premium** (~1.25×) to populate it, and cache entries **expire** (TTLs are often ~5 minutes) — so a low-traffic workload can miss repeatedly and end up worse off. And the only way to know which case you're in is to **instrument `cache_read_input_tokens`**. An unmeasured caching strategy is a belief, not a result. (`content/05` §4)

---

### Scoring

| Score | Reading |
|---|---|
| **8–9** | You can price a workload and challenge a vendor quote. Go do it on something real this week. |
| **6–7** | Solid. Re-read `content/05` — the multipliers are where the money actually is. |
| **4–5** | The vocabulary landed; the cost model hasn't yet. Work through `exercises/lab.md` — running the estimator fixes this faster than re-reading. |
| **≤ 3** | Start again at `content/00-overview.md` and do the lab alongside. Everything in the session hangs off one sentence: *cost scales with tokens, not with requests.* |
