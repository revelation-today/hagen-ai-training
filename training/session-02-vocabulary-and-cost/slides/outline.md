# Slides — Session 2: The Vocabulary, and the Cost Meter

Slide-by-slide spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, accessibility, licence-footer rules — not restated here). Target: **16 content slides** + title + agenda + Q&A + resources = **20 slides**, ~45 min. Speaker notes go in the Notes pane, never on the slide. Every headline is a claim, not a label.

Licence quick-reference for this deck:
- **SLIDE-SAFE (embed + attribute):** `tiktoken` (MIT) code snippets; Hugging Face tokenizers/course (Apache-2.0). All Mermaid below is **original to this course** and safe to render.
- **LINK-ONLY (never embed; live-demo / link on the resources slide):** the OpenAI tokenizer web UI (demo it, don't screenshot the interface); all vendor pricing pages; the O'Reilly source decks; the Cisco `mlfund` deck — **`Cisco Confidential`, never reproduce anything from it.**

> ### ⚠️ Two build notes specific to this deck
> 1. **Every price on every slide is illustrative and must be refreshed on the delivery date.** Put the marker *"Illustrative — verify at delivery"* in the footer of slides 11, 12, 13, 14, 15 and 16. Update `content/04`, `content/05` and these slides together, or they will disagree in front of the room.
> 2. **Slide 8 is a live demo with a network dependency.** Build slide 8b as a static fallback table of the six token counts. Two minutes of work; saves the segment.

---

## Slide 1 — Title

- **On-slide text:** "The Vocabulary, and the Cost Meter" · Session 2 of 16 · Block: *Understand it* · AI Training Series.
- **Speaker notes:** Two halves today: the words everyone uses loosely, and the bill nobody forecasts correctly. They connect through one term — the token — which turns out to be the unit of reading, writing, and billing all at once. This is the reference session; people come back to it.
- **Visual:** Series title layout.
- **Source/licence:** none.

## Slide 2 — Agenda

- **On-slide text:** The nested vocabulary · Model, training, inference, parameters · The token · What it costs · The context multiplier · Q&A.
- **Speaker notes:** Mirror the minute-budget in `README.md`. Flag the pivot: everything before the token is definitions; everything after is money. Tell them the one sentence they're getting at minute 43, so they listen for it.
- **Visual:** Agenda layout matching the README table.
- **Source/licence:** none.

## Slide 3 — Hook: this sentence costs more in German (live demo)

- **On-slide text:** `platform.openai.com/tokenizer` · One sentence in English → ~10 tokens · The same sentence in German → ~17 · One log line → ~39.
- **Speaker notes:** Open the tokenizer live. Paste the English sentence, read the count. Paste the German. Paste a real log line with a timestamp and a hash. Then say the line the session turns on: *"Every one of those numbers is a line item."* Don't explain anything yet — 90 seconds, then move. **Fallback:** switch to slide 8b if there's no network.
- **Visual:** Live browser. **Do not screenshot the tool's UI onto the slide.**
- **Source/licence:** OpenAI tokenizer — **LINK-ONLY, live demo only.**

## Slide 4 — Four words, one containment relationship

- **On-slide text:** AI ⊃ Machine Learning ⊃ Deep Learning ⊃ LLMs · None of the arrows run backwards · Most working AI sits in the *branches*, not the LLM box.
- **Speaker notes:** These get used as synonyms and are not. Walk the nesting once. Then point at the dotted branches — rule-based AI, classical ML, vision networks — because that is where most deployed systems actually live. The useful question when someone says "we're adding AI" is *which layer?*
- **Visual:** The nesting graph from `content/01` §1:
  ```mermaid
  graph TD
      AI["<b>Artificial Intelligence</b>"] --> ML["<b>Machine Learning</b><br/>rule learned from data"]
      ML --> DL["<b>Deep Learning</b><br/>multi-layer neural nets"]
      DL --> LLM["<b>LLMs</b><br/>predict the next token"]
      AI -.-> RB["Rule-based AI<br/><i>not ML</i>"]
      ML -.-> CL["Trees, forests, k-means<br/><i>not deep</i>"]
      DL -.-> CNN["Vision networks<br/><i>not language</i>"]
  ```
- **Source/licence:** original diagram. Alt text: a four-level containment tree with three side branches naming systems that stop at each level.

## Slide 5 — The same problem, at all four layers

- **On-slide text:** Ticket triage as: hand-written rules · a fitted SLA classifier · a network reading raw text · an LLM that summarises and drafts · Descending buys generality, costs auditability.
- **Speaker notes:** One example, four implementations — this is what makes the nesting concrete rather than a Venn diagram. Land the discipline note: a regex you can read, version, and diff is a *better engineering artefact* than an opaque model that's three points more accurate — unless those three points are worth the loss. Session 5 makes this concrete; Session 13 makes it uncomfortable.
- **Visual:** The four-row table from `content/01` §2, trimmed to columns *Layer / Implementation / Who wrote the rule / Typical failure*.
- **Source/licence:** original.

## Slide 6 — A model is fitted numbers, not a program

- **On-slide text:** Our SLA classifier = **5 numbers** + a weighted sum · Nobody wrote `+0.84` · Running it is 4 multiplications.
- **Speaker notes:** Show the five weights. This is the whole demystification: a model is a pile of numbers plus arithmetic. It scales identically to a trillion parameters — same idea, more numbers. And note how cheap it is to *run* — which sets up the next slide.
- **Visual:** The five-weight table from `content/02` §1. Optionally the 6-line `predict_breach_probability` snippet.
- **Source/licence:** original.

## Slide 7 — Training is the vendor's bill; inference is yours

- **On-slide text:** **Training** — rare, expensive, needs labels · **Inference** — constant, cheap per call · Calling a hosted LLM = **inference only** · "Training costs $100M" is true *and irrelevant to your invoice*.
- **Speaker notes:** Kill the commonest cost misunderstanding in the room: people read the training headlines and assume usage is expensive. Training already happened, at the vendor's expense. Then kill the opposite error: "per call it's fractions of a cent, so cost isn't a concern." Tiny × huge × a context that grows every turn is a real number. Both errors get corrected in the next twenty minutes. Also flag hyperparameters here in one line: human-set, pre-training, and — for this room — **configuration items**. Unrecorded hyperparameters mean an irreproducible model.
- **Visual:** The training/inference Mermaid from `content/02` §2 plus the comparison table.
- **Source/licence:** original.

## Slide 8 — A token is ¾ of a word — until it isn't

- **On-slide text:** 1 token ≈ **0.75 words** ≈ 4 characters · 1,000 words ≈ **1,300 tokens** · German 1.8–2.2 tok/word · Code 1.8–2.5 · JSON 2.0–3.0 · Log lines 2.5–4.0.
- **Speaker notes:** Give them the anchor first, then the table that breaks it. The operational punchline: the *same information* costs different amounts depending on how you serialise it — 50 records as JSON can cost double the same 50 records as a markdown table. That is free money and requires no cleverness. Caveat out loud: every vendor has a different tokeniser, so these are planning numbers, not billing numbers.
- **Visual:** The tokens-per-word table from `content/03` §3 (all seven rows) + the page/document anchors.
- **Source/licence:** original. Ratios are our own measurements — reproducible with the slide 9 code.

## Slide 8b — Fallback: the six token counts (build this, use only if offline)

- **On-slide text:** Static table of the six demo strings and their token counts, from `exercises/lab.md`.
- **Speaker notes:** Only shown if slide 3's live demo fails. Read the counts, make the same point, move on. Never present both.
- **Visual:** Text table. No screenshots of the tokenizer UI.
- **Source/licence:** counts generated with `tiktoken` (MIT) — attribute in footer.

## Slide 9 — Count them yourself: five lines of Python

- **On-slide text:** `import tiktoken` · `len(enc.encode(text))` · Prose 1.17 tok/word · German 2.38 · JSON 3.40 · Log line 3.90.
- **Speaker notes:** Show the snippet, not the theory. The qualitative pattern is stable across every tokeniser; the digits are not. Then the practical rule: estimate with `tiktoken`, **bill from the API's reported usage block**. Tell them to instrument token counts on day one of any pilot — prices drift, token counts don't, and tokens are what tells you whether an optimisation worked.
- **Visual:** The `count_tokens` snippet from `content/03` §4 with its expected output comment.
- **Source/licence:** **`tiktoken` — MIT. SLIDE-SAFE.** Footer: "Code uses tiktoken (MIT)."

## Slide 10 — Output tokens cost more because each one is a full pass

- **On-slide text:** Input: processed once · Output: **one token at a time**, whole model re-run each time · 500-token answer = **500 forward passes** · Hence ~4–5× the price.
- **Speaker notes:** This is the mechanism, and it's worth thirty seconds because it makes the price ratio stop looking arbitrary. The model doesn't write a paragraph; it writes a token, appends it, and runs again. Then the conclusion the whole second half rests on: tokens are the closest cheap proxy for GPU-seconds, so tokens are what gets charged. A *request* is not a unit of anything the vendor spends.
- **Visual:** The three-input billing Mermaid from `content/03` §6.
- **Source/licence:** original.

## Slide 11 — Same task, three tiers, 60× spread

- **On-slide text:** Triage one ticket: 1,400 in / 300 out · Frontier **$0.0435** · Workhorse **$0.0087** · Small **$0.000725** · 2,000/month → **$87 / $17.40 / $1.45**.
- **Speaker notes:** Walk one row of arithmetic out loud so they see it's just multiplication. Then the headline: sixty-fold, for the same task. No prompt optimisation closes a 60× gap — so the *first* question is never "how do I shorten this?", it's "does this task need the frontier model?" Ticket triage is classify-and-summarise; that's a small-model task. Answer it by testing 200 real tickets across all three tiers with blind human grading — not by assuming.
- **Visual:** The two cost tables from `content/04` §2.
- **Source/licence:** original. **Footer: "Illustrative — verify at delivery."**

## Slide 12 — Output is 18 % of the tokens and 52 % of the bill

- **On-slide text:** 300 of 1,700 tokens = 17.6 % of traffic · at 5× price = **51.7 % of cost** · Cutting 150 **output** tokens saves 26 % · Cutting 150 **input** tokens saves 5 %.
- **Speaker notes:** Same 150 tokens, five times the effect depending which end they come from. So "Be concise. Maximum 100 words." is a cost control, not a style preference. Note it holds identically across all three tiers because the 5:1 ratio is the same — this is structural, not a quirk of one vendor.
- **Visual:** A simple two-bar comparison: share of tokens vs. share of cost. Label both bars; don't rely on colour.
- **Source/licence:** original. **Footer: "Illustrative — verify at delivery."**

## Slide 13 — THE INSIGHT: cost scales with tokens, not requests

- **On-slide text:** **2,000 requests/month in all three cases** · X: ticket only → **$17.40** · Y: + a 40-page spec → **$173.40** · Z: same spec, cached → **$33.00** · A per-request model predicts all three are equal.
- **Speaker notes:** Slow down — this is the slide the session exists for. Identical request counts, 10× apart. Ask the room where their instinct came from: transactions, tickets, seats, API calls — everything else they budget for charges per *event*, and events are countable in advance. This isn't. Then the governance consequence: a pull request that adds "include the last 10 tickets for context" changes no interface, adds no requests, passes review, and multiplies the bill. **An LLM design change is a cost change.**
- **Visual:** The three-branch Mermaid from `content/04` §3:
  ```mermaid
  flowchart TD
      R["2,000 requests/month<br/>(identical in all three)"]
      R --> X["<b>X</b> · ticket only<br/>$17.40"]
      R --> Y["<b>Y</b> · + 26k-token spec<br/><b>$173.40</b> — 10×"]
      R --> Z["<b>Z</b> · same spec, cached<br/>$33.00"]
  ```
- **Source/licence:** original. **Footer: "Illustrative — verify at delivery."**

## Slide 14 — Every turn re-sends the whole conversation

- **On-slide text:** The API is **stateless** · "Memory" = re-sending the transcript · Your 3rd message: 200 tokens typed, **1,900 billed**.
- **Speaker notes:** This is the mechanism behind the next slide's curve, and most people have never been told it. There is no session on the server. The illusion of a chatbot remembering you is produced entirely by you re-sending the transcript — and you're billed for it every time. Show the turn-3 stack: system prompt + two full exchanges + the new message.
- **Visual:** The turn-3 composition Mermaid from `content/05` §1.
- **Source/licence:** original.

## Slide 15 — Conversation cost grows with n², not n

- **On-slide text:** Turn 1: 700 input tokens · Turn 20: **12,100** · Cumulative over 20 turns: **128,000** vs. a per-request estimate of 14,000 — **9×** · 1,000 such chats/mo: **$504**, budgeted **$162**.
- **Speaker notes:** The bars are what you pay; the line is what a per-request forecast predicts. Nobody did anything wrong here — the estimate was built on the wrong unit. Add the second-order point: the last turn costs ~17× the first, so an open-ended chat with no turn limit has unit economics that get *worse* the more engaged the user is. That's backwards, and it's a product decision, not an infrastructure one.
- **Visual:** The `xychart-beta` from `content/05` §2 (bars = actual cumulative input, line = naive estimate). If the builder can't render xychart, use the turn table instead — it carries the same point.
- **Source/licence:** original. **Footer: "Illustrative — verify at delivery."**

## Slide 16 — Three multipliers that don't change your request count

- **On-slide text:** **Documents** — 26k tokens/call, re-sent every turn · **RAG** — `k` chunks × chunk size, set in a config file · **Agents** — 1 user request → **8 billed calls → ~13× cost**.
- **Speaker notes:** All three are invisible to a request-based forecast. RAG is the sneakiest: retrieval quality improves as you raise `k`, so there's constant pressure upward, and `k` lives in a config file no cost reviewer opens. Treat retrieval parameters as cost-bearing configuration items. Agents are the sharpest: one user action, eight model calls, each carrying the whole accumulated trace. This is where forecasts fail hardest.
- **Visual:** The agent `sequenceDiagram` from `content/05` §3, plus the RAG cost table (3 rows).
- **Source/licence:** original. **Footer: "Illustrative — verify at delivery."**

## Slide 17 — Six levers, in order of leverage

- **On-slide text:** 1. Change tier (**60×**) · 2. Cache the stable prefix (**~90 %** off repeated input) · 3. Cap the output (**5×** unit price) · 4. Trim the input · 5. Batch (**50 %**) · 6. Don't call the model.
- **Speaker notes:** The order is not arbitrary — it's the order of the multipliers. Two honesty notes: tiering down must be *measured*, because a cheaper model that's wrong more often costs more in rework than it saved in inference; and a sliding window over history isn't free, it makes the assistant visibly forget things. Both are cost/quality trades that deserve a documented decision, not a silent config change.
- **Visual:** The levers flowchart from `content/05` §4.
- **Source/licence:** original.

## Slide 18 — Put the stable content first. A timestamp at the top costs you the cache.

- **On-slide text:** Caching works on a **prefix** · ❌ `[timestamp][system][spec][ticket]` → 0 % hits · ✅ `[system][spec] | [timestamp][ticket]` → ~90 % off 26,500 tokens · It's a **reorder**, not a rewrite.
- **Speaker notes:** The single most actionable thing in this session. One variable token early invalidates everything after it. Show both orderings side by side — this lands instantly with anyone who's built a prompt template. Then the caveats: TTLs are short (often ~5 min), low-traffic workloads miss and pay the write premium, and short prompts don't qualify. Finish with: instrument `cache_read_input_tokens`. **An unmeasured caching strategy is a belief, not a result.**
- **Visual:** The ❌/✅ prompt-ordering block from `content/05` §4, Lever 2. Use ❌/✅ *and* the words "broken"/"cached" — never colour alone.
- **Source/licence:** original. **Footer: "Illustrative — verify at delivery."**

## Slide 19 — Q&A / discussion

- **On-slide text:** "Where in your work would a **per-request** cost estimate have been wrong — and by how much?" · Poll: *Have you ever seen an AI bill you couldn't explain?* Yes / No / We don't measure it.
- **Speaker notes:** Run the seed question from `exercises/discussion.md`. The "we don't measure it" poll option is the honest one and usually wins — use it to land the instrumentation point one final time. If the room is quiet, price *their* workload live using the estimator; five minutes with real numbers beats any slide here.
- **Visual:** Discussion/poll layout.
- **Source/licence:** none.

## Slide 20 — Resources & credits

- **On-slide text:** `tiktoken` (MIT) · Hugging Face tokenizers (Apache-2.0) · Live demo: `platform.openai.com/tokenizer` · **Pricing: pull fresh from vendor pages on the day** · Full list: `resources/sources.md`.
- **Speaker notes:** Two SLIDE-SAFE code sources, attributed. Everything else — the tokenizer UI, vendor pricing, the source decks — is link-only. Say out loud that every price in the deck is illustrative and dated, and that `resources/sources.md` carries the refresh checklist.
- **Visual:** Resources & credits layout with licence attributions.
- **Source/licence:** as listed; SLIDE-SAFE items attributed, LINK-ONLY items linked only.

---

### Build checklist (this deck)

- [ ] 16 content slides (3–18, incl. 8b as an unused fallback); none over 6 bullets; every headline is a claim.
- [ ] **Every price slide (11–16, 18) carries the footer "Illustrative — verify at delivery," and the numbers match `content/04`–`05` after refresh.**
- [ ] Slide 8b built and tested as the offline fallback for slide 3.
- [ ] No screenshot of the OpenAI tokenizer UI anywhere in the deck.
- [ ] **No content, figure, or wording from the Cisco `mlfund` deck.** It is `Cisco Confidential` and excluded.
- [ ] Only slides 9 and 8b embed sourced material (`tiktoken`, MIT) — footer attribution present.
- [ ] All diagrams rendered from the original Mermaid above; alt text on every one; greyscale-safe; 18 pt minimum.
- [ ] ❌/✅ on slide 18 backed by text labels, not colour alone.
- [ ] Runs in ~45 min rehearsed. Slide 3 (hook), slide 13 (the insight) and slide 18 (the lever) get the most air.
