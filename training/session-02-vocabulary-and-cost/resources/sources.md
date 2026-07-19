# Sources — Session 2

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / BSD / MIT / Apache / public-domain / standards body. May derive slide and content text/figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NC / ND / vendor / internal / confidentiality-marked. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY.

> ### Authoring status of this session
> **The entire cost half of Session 2 (`content/03` §3–6, `content/04`, `content/05`) is authored for this course.** No source deck in the corpus covers token economics; `AI_input.md` §3 lists "cost / latency / model-selection engineering" as *thin*, and the training proposal lists token cost as one of four goals with **no source material at all**.
>
> Practically this means: **there is nothing here to license-launder** — every diagram, table, worked example, and number is ours. It also means **the numbers are ours to keep current**, and they will go stale faster than anything else in the series.

---

## Slide-safe (embeddable with attribution)

**#1 — `tiktoken` (OpenAI's byte-pair-encoding tokeniser library).**
- URL: `https://github.com/openai/tiktoken`
- Licence: **MIT.** Permissive; derivation and redistribution permitted with the licence notice.
- Used for: all token-counting code in `content/03` §4, `exercises/lab.md` Parts 2–5, and slide 9. Also used to generate the token counts in the slide-8b offline fallback table.
- **Reuse verdict: SLIDE-SAFE.** Attribute on-slide as "Code uses tiktoken (MIT)."
- *Verify at delivery:* confirm the current encoding names. `o200k_base` and `cl100k_base` are the ones used here; encoding names are added over time and the lab should reference one that exists.

**#2 — Hugging Face `tokenizers` library and the Hugging Face NLP/LLM course.**
- URLs: `https://github.com/huggingface/tokenizers` · `https://huggingface.co/learn`
- Licence: **Apache-2.0** (library); the course materials are Apache-2.0 licensed. Attribution required.
- Used for: the subword-tokenisation explanation in `content/03` §1–2 (stated in our own words), and as the recommended vendor-neutral alternative for anyone who wants to inspect a non-OpenAI tokeniser.
- **Reuse verdict: SLIDE-SAFE.** Attribute if any wording or figure is derived. Nothing is currently reproduced verbatim.
- *Verify at delivery:* Hugging Face reorganises course URLs periodically; check the link resolves.

---

## Link-only (reference / assign / live-demo — never embed)

**#3 — OpenAI Tokenizer (web tool).**
- URL: `https://platform.openai.com/tokenizer`
- Licence: **all-rights-reserved** (OpenAI website UI). Free to use; the *interface* is not licensed for reproduction.
- Used for: **the live demo** on slide 3 and `exercises/lab.md` Part 1.
- **Reuse verdict: LINK-ONLY.** Demo it live in the browser. **Do not embed screenshots of the tool's UI on a slide.** The offline fallback (slide 8b) is a *text table of counts we generated ourselves with `tiktoken` (#1)* — that is our own data, not their interface.
- *Verify at delivery:* confirm the page still loads without a login and that the sample strings produce the expected counts. This is the session's only hard network dependency.

**#4 — Vendor pricing pages (OpenAI, Anthropic, Google, and any others quoted).**
- URLs: `https://openai.com/api/pricing/` · `https://www.anthropic.com/pricing` · `https://ai.google.dev/pricing` *(check current locations — vendors move these)*
- Licence: **all-rights-reserved.** Vendor marketing pages.
- Used for: **nothing is reproduced.** Every price table in `content/04` and `content/05` is our own illustrative construction, labelled as such. Vendor pages are the authority you check against on the delivery date.
- **Reuse verdict: LINK-ONLY.** Link on the resources slide. Do not screenshot a pricing table onto a slide.
- ⚠️ **This is the session's primary currency risk.** See the refresh checklist below.

**#5 — `LLM System Safety and Security` (Nield, T. — O'Reilly live training).**
- Source deck, "Understanding LLMs" progression (slides 6–9) and the summary claim.
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- Used for: the *"autocomplete on steroids — a pattern-spotting and matching engine, not a search engine looking up facts"* framing, carried forward from Session 1 into `content/01` §6. Delivered in our own words; the phrase is common parlance. Attribute the *framing* verbally; do not reproduce the deck's slides.

**#6 — `Mastering the Fundamentals of AI and ML` (Barton, R. & Henry, J.).**
- Licence: **`Cisco Confidential`** — every slide carries a Cisco MSIP sensitivity banner; both authors are Cisco employees.
- **Reuse verdict: LINK-ONLY, and stronger than that — EXCLUDED.** Per `AI_input.md` §1, using another company's confidentiality-marked internal material to train Qualcomm employees is not defensible. **Nothing from this deck appears in this session: no content, no wording, no figures, no examples, no term list.**
- Used for: **structure inspiration only** — the observation that a complete foundations curriculum defines its vocabulary early, and that the AI/ML/DL taxonomy is standard material appearing in three separate decks in the corpus. That observation is a fact about curriculum design, not content taken from the deck. The AI ⊃ ML ⊃ DL ⊃ LLM nesting is textbook-standard and is written here from first principles against our own running example.
- The `platform.openai.com/tokenizer` tool is also listed in that deck's asset index. It is a **publicly available third-party tool**, not Cisco content; our use of it is independent and cites OpenAI (#3).

**#7 — `Deep Learning for Beginners`, Days 1–3 (Nield, T. — O'Reilly).**
- Licence: all-rights-reserved. **LINK-ONLY.**
- Used for: the *classical programming vs. machine learning* inversion (`content/01` §4) — a standard framing that appears in several of the corpus decks and is stated here in our own words with our own diagram. Also the source of the **corrected** deep-learning definition defect; see below.

**#8 — Provider documentation on prompt caching, batch processing, and context windows.**
- Vendor documentation pages (locations move; find via each vendor's API docs).
- Licence: all-rights-reserved vendor documentation. **LINK-ONLY.**
- Used for: the *shape* of the caching model in `content/05` §4 — prefix-based, cache-write premium, cache-read discount, TTL, minimum cacheable length. All figures given are **illustrative and rounded**; the mechanics are described in our own words. **The exact discount rate, write premium, TTL, and minimum length differ per vendor and must be verified.**

---

## Currency register — what must be re-verified before delivery

This session ages faster than any other in the series except 11 and 15. Work through this list on the delivery date and update `content/04`, `content/05`, `exercises/lab.md`, `exercises/quiz.md` and `slides/outline.md` **together** — they share numbers and will disagree if you update one.

| # | Item | Where it appears | How to verify |
|---|---|---|---|
| 1 | **Tier prices** (A $15/$75, B $3/$15, C $0.25/$1.25 per 1M tok) | `content/04` §1; every downstream table; lab; quiz; slides 11–16 | Vendor pricing pages (#4). If the ratios still hold, only the absolutes need editing. |
| 2 | **Output : input price ratio** (~5×) | `content/04` §2; quiz Q6 | Vendor pricing pages. Stable at 4–5× for over a year, but check — reasoning-model tiers sometimes differ. |
| 3 | **Tier spread** (~60× A→C) | `content/04` §2; slide 11 | Recompute from #1. |
| 4 | **Cache-read discount** (~90 % off) and **cache-write premium** (~1.25×) | `content/05` §4; quiz Q9 | Vendor caching docs (#8). |
| 5 | **Cache TTL** (~5 minutes) and minimum cacheable length | `content/05` §4 | Vendor caching docs. Several vendors now offer longer, separately-priced TTLs. |
| 6 | **Batch discount** (~50 %) | `content/05` §4 | Vendor batch/async endpoint docs. |
| 7 | **Context window sizes** (~128k–1M) | `content/05` §1 | Vendor model pages. Moves upward frequently. |
| 8 | **Long-context surcharge thresholds** | `content/05` §5 | Vendor pricing pages. Whether it applies to the whole call or only the excess matters — check. |
| 9 | **Model names per tier** | Anywhere a named model is used as an example | Deliberately kept generic (A/B/C) so the lesson survives. If you name models on the day, name current ones. |
| 10 | **`tiktoken` encoding names** (`o200k_base`, `cl100k_base`) | `content/03` §4; lab | `tiktoken` repo (#1). |
| 11 | **Tokenizer web tool availability** | Slide 3; lab Part 1 | Load the page; confirm no login required. Build slide 8b regardless. |
| 12 | **Tokens-per-word ratios** by content type | `content/03` §3; slide 8 | Re-run the lab's Part 2 cell against the current encoding. |

**Rule of thumb for the refresh:** the *structure* of this session — input vs. output, quadratic conversations, prefix caching, tokens-not-requests — is a property of how the technology works and will outlast several price cycles. The *digits* are perishable. Update the digits; trust the structure.

---

## Further reading (LINK-ONLY, high quality — assign, don't slide)

| Topic | Suggestion | Why |
|---|---|---|
| How subword tokenisation actually works | The Hugging Face NLP course chapter on tokenizers (#2 — this one is Apache-2.0, so it is also slide-safe) | The clearest free explanation of BPE and WordPiece, with runnable code. |
| Seeing tokenisation, not reading about it | The OpenAI tokenizer (#3) with the segment-colouring view on | Five minutes with your own text beats any explanation. |
| Why long context is computationally expensive | Session 9 of this course (attention and O(n²) scaling) | The mechanism behind the latency behaviour noted in `content/05` §5. |
| Prompting cheaply and well | Session 10 of this course | The "a well-prompted cheap model can match an expensive one" claim, made properly and with evidence. |
| What you're allowed to put in a context window | Session 14 of this course | Context *is* data egress. The cost conversation and the data-handling conversation are the same conversation. |
| Current prices, always | The vendor pricing pages (#4) | The only authority. Everything in this session is illustrative. |

---

### Corrections carried into this session (per `../../../AI_input.md` §6)

**Defect #13 — the deep-learning definition.** One source deck defines deep learning as "more than one hidden layer" and then teaches a running example with exactly **one** hidden layer — so by its own definition, the course's own network is not deep learning.

We do not reproduce that definition as fact. `content/01` §5 gives the honest version instead: **"deep" is a loose, conventional label meaning "enough stacked layers that the network learns useful intermediate representations by itself" — not a defined threshold.** The fuzziness is stated out loud rather than hidden, because a technical Qualcomm audience will probe the definition, and being caught quoting a threshold that the source itself violates would cost more credibility than the admission does.

**Defect #6 — SVM.** The excluded Cisco deck names SVM as a stated objective and as *the* multiclass algorithm, with zero content slides. This session's vocabulary does not mention SVMs at all; classical-ML methods are Sessions 4–5, which are rebuilt from scikit-learn (BSD-3). No gap is inherited here.
