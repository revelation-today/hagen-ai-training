# Session 2 — The Vocabulary, and the Cost Meter

**Block:** Understand it · **Goal covered:** 2 · **Format:** 45 min content + 15 min Q&A

---

## One-paragraph summary

This is the reference session — the one people come back to. It has two halves that look unrelated and are not. The first half builds the vocabulary **once, cleanly**: AI ⊃ machine learning ⊃ deep learning ⊃ LLMs, plus *model*, *token*, *training vs. inference*, and *parameters vs. hyperparameters* — every term defined against **one running example** (an automated defect-ticket triage system) so the words attach to something concrete instead of floating. The second half is the half no source deck covers and that this audience will feel first: **what AI actually costs.** The token turns out to be the unit of three different things at once — how the model reads, how it generates, and how you are billed. From that single fact everything else follows: why output tokens cost several times what input tokens cost, why a chat conversation gets more expensive *per turn* the longer it runs, why attaching a document or wiring up an agent multiplies your bill without changing your request count, and why prompt caching is the biggest lever you have. The headline, and the thing that catches out anyone with a per-transaction billing instinct: **cost scales with tokens, not with requests.**

## Audience & level

Qualcomm release / problem / configuration managers and developers. No machine-learning background assumed. There is Python here (`tiktoken` token counting, a cost-estimator function), but **non-coders can complete the session** — the live tokenizer demo and the cost tables carry the same lesson without running anything.

Role-specific note: the cost half is not trivia. If your team is drafting a business case for an AI tool, negotiating with a vendor, or sizing a pilot, **the token model is the thing that makes an estimate right or wrong by an order of magnitude.** Release and configuration managers in particular should leave able to challenge a "cost per request" projection.

## Learning objectives

After this session a participant can:

1. **Draw** the AI ⊃ ML ⊃ DL ⊃ LLM nesting and place a given system correctly in it — including systems that are AI but not ML.
2. **Distinguish** training from inference, and parameters from hyperparameters, using one worked example, and say who pays for each and when.
3. **Explain** what a token is, estimate a token count from a word count (≈ 1.3 tokens per English word), and predict which inputs tokenise badly (code, German, JSON, IDs).
4. **Compute** the cost of a given workload from input tokens, output tokens, and a published price table — and explain why output tokens dominate the bill despite being a minority of the tokens.
5. **Explain** why a stateless chat API re-sends the whole conversation each turn, and show that the cumulative cost of an *n*-turn conversation grows roughly with *n²*.
6. **Identify** the main cost levers — prompt caching, model tiering, context trimming, output-length control — and say when each applies and when it backfires.

## Prerequisites

**Session 1** ("What AI Is, and How It Relates to Human Thinking"). Session 2 is the direct consequence of Session 1's closing model: *a model that predicts tokens is a model you pay for by the token.* No coding prerequisite. If you want to run the lab you need a browser (Google Colab) — see `exercises/lab.md`.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook** | Live: paste one sentence into `platform.openai.com/tokenizer`. Then paste the same idea as code, and as German. Watch the count jump. "Everything in this session is downstream of that number." |
| 3–10 min | **The nested vocabulary** | AI ⊃ ML ⊃ DL ⊃ LLM against one running example (defect-ticket triage). Includes the counter-example: AI that is not ML. |
| 10–17 min | **Model, training, inference, parameters** | What a model *is*; training vs. inference and who pays for which; parameters vs. hyperparameters. |
| 17–23 min | **The token, properly** | Subword tokenisation; ≈ ¾ word per token in English; what tokenises badly and why it costs you. |
| 23–31 min | **The bill: input ≠ output** | Input and output priced separately. The three-tier worked example: same task, 60× price spread. Output is 18 % of the tokens and half the cost. |
| 31–39 min | **The context window as a cost multiplier** | Stateless APIs re-send everything. The turn-by-turn growth table and the *n²* diagram. RAG, long documents, agents. |
| 39–43 min | **The levers** | Prompt caching (the big one), model tiering, trimming, capping output. When each backfires. |
| 43–45 min | **The one insight** | Cost scales with **tokens**, not with **requests**. Restate; hand over. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. Seed: *"Where in your work would a per-request cost estimate have been wrong — and by how much?"* |

**Is 45 minutes honest?** It is tight but it fits, because the two halves reinforce rather than compete. Discipline points: (a) do **not** explain how tokenisation algorithms are trained — the demo does the work; (b) do **not** drift into prompting technique, that is Session 10. If you run long, cut the RAG/agent multiplier detail (`content/05` §4) to a single sentence and point at the reading — it returns in later sessions anyway.

## Materials & tools

- Slides: `slides/outline.md` → deck built per `../powerpoint_instructions.md`.
- **Live demo (network required):** the OpenAI tokenizer at `https://platform.openai.com/tokenizer`. Free, no login, browser only. **Build a fallback slide** — a text-only table of the counts you intend to show — in case the room has no network or the page changes. Do **not** embed screenshots of the tool's UI on a slide without attribution; describe it or show it live (see `resources/sources.md`).
- **Lab (optional, ~25 min):** Google Colab notebook using `tiktoken` (MIT) to count tokens and price a workload. JupyterLite fallback noted in `exercises/lab.md`.
- Calculator or spreadsheet for the Q&A exercise — participants price their own workload.

## Source & licence note

| Source | Role in this session | Reuse verdict |
|---|---|---|
| **`tiktoken`** (OpenAI tokeniser library) | Code examples for token counting | **SLIDE-SAFE** (MIT — attribute) |
| **Hugging Face `tokenizers` / course** | Alternative tokeniser code; subword-tokenisation explanation | **SLIDE-SAFE** (Apache-2.0 — attribute) |
| **OpenAI tokenizer web tool** | The live demo | **LINK-ONLY** — demo it, don't screenshot the UI onto a slide |
| **Vendor pricing pages** (OpenAI, Anthropic, Google) | The real numbers, pulled at delivery | **LINK-ONLY** — link on the resources slide; our tables are *illustrative* |
| **LLM System Safety and Security** deck (Nield, O'Reilly) | "Autocomplete on steroids" framing carried from Session 1 | **LINK-ONLY** — paraphrase |
| **Mastering the Fundamentals of AI and ML** (Barton & Henry, Cisco) | Structure inspiration only (a foundations curriculum's term list) | **LINK-ONLY — `Cisco Confidential`. Never reproduce any content or figure.** |

**Authoring note:** the *entire cost half* of this session is written for this course. No source deck covers token economics; the proposal flags it as a full-authoring item. That is a feature — it means there is nothing here to license-launder, but it also means **the numbers are ours to keep current.**

> ### ⚠️ Currency warning — read before delivering
> **Every price in this session is illustrative and must be re-verified on the day.** Model names, tiers, and per-million-token prices move several times a year, and the ratios between tiers move too. Each price table carries the marker **"verify at delivery (as of mid-2026: …)"**. Pull fresh figures from the vendors' own pricing pages before you present, and update `content/04`, `content/05`, and the slides together. Full provenance and licence verdicts: `resources/sources.md`.

> ### Correction carried into this session (per `../../../AI_input.md` §6, defect #13)
> One source deck defines deep learning as "more than one hidden layer" while its own running example has exactly **one** hidden layer — so by its own definition the course's network is not deep learning. We do not repeat that. `content/01` gives the honest version: **"deep" is a loose, conventional label, not a defined threshold**, and says so out loud. A technical audience will probe this definition; being straight about its fuzziness costs nothing and buys credibility.
