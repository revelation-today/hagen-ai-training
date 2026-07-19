# Session 10 — Prompting I: The Craft

**Block:** Working with LLMs · **Goal:** 5 (prompting) · **Format:** 45 min content + 15 min Q&A · **Hands-on:** yes — a short Python lab (`exercises/lab.md`), runnable in Colab

---

## Summary

Most people's prompting practice is *typing something, reading the answer, and shrugging*. This session replaces that with a technique: **define the objective → draft → test → refine → iterate → evaluate**, run as a loop, against a small set of real examples, with the prompt treated as a versioned artifact that can regress. We give the room a **working task-type taxonomy** (11 categories, each with its own design principles), then teach the vocabulary the 2024-era source deck predates entirely — **zero-shot vs. few-shot, chain-of-thought, system messages, delimiters, self-critique** — and the piece that makes prompting useful inside real tooling: **structured output** (JSON schema, constrained decoding, function calling). We close on the engineering lever this audience will actually feel: **a cheap model, well-prompted, can match an expensive one — and you can measure it.** Every technique here comes with complete, verbatim before/after prompts on tasks this room does: drafting release notes from commits, triaging an incident ticket, reviewing a config diff.

The honest framing throughout: **these nuances are found through testing, not guessing.** A prompt you have not tested is folklore.

## Audience & level

Qualcomm release / problem / configuration managers and developers. Everyone in the room already uses an LLM chat interface; almost nobody in the room tests their prompts. The session is designed to work at two levels at once:

- **Non-coders** get the cycle, the taxonomy, the before/after prompt pairs, and the cost argument — all usable in a chat window on Monday.
- **Developers** additionally get the API-level material: system messages as a separate channel, structured output via JSON schema and tool schemas, and a minimal eval harness.

Prior sessions supply the mechanism (why the model behaves this way); this session supplies the craft.

## Learning objectives

By the end, a participant can:

- **Run** the prompt-engineering cycle on one of their own tasks — define a checkable objective, draft, test against ≥5 real examples, refine, and stop deliberately rather than when tired.
- **Classify** a task using the 11-type taxonomy and **select** the matching techniques from a decision table, instead of reaching for the same prompt shape every time.
- **Distinguish** zero-shot from few-shot, and explain when adding examples helps, when it costs more than it returns, and when it actively harms.
- **Explain** why "let's think step by step" was a 2023 workaround and what replaced it — chain-of-thought as a *model parameter* on reasoning models — and decide when reasoning is worth its latency and token cost.
- **Write** a prompt that uses a system message, explicit delimiters separating instructions from data, and a self-critique pass — and say what each element is defending against.
- **Get reliable JSON out of a model**, and explain the difference between *asking* for JSON (instruction-following) and *guaranteeing* it (constrained decoding / tool schema).
- **Make and defend** the cost argument: a smaller model, well-prompted, matching a larger one — including the caveat that you must compare at equal token spend.

## Prerequisites

- **Session 1** (what AI is; hallucination as a mechanism, not a bug to be scolded out of the model).
- **Session 9** (tokens, context window, next-token prediction, temperature). The cost arithmetic in `content/07` assumes you know what a token is.
- Helpful but not required: Python literacy for the code blocks and the lab. Every code example is also explained in prose, and every prompt is given in full so a non-coder can paste it into a chat window.
- **Session 13–13 are downstream of this one:** the "separate instructions from data" habit taught here is the first line of defence against prompt injection, which Session 14 attacks properly.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–4 min | **Hook** | The same release-notes task, two prompts. Same model, same commits. One output is unusable; the other ships. Nothing changed but the prompt. |
| 4–11 min | **The cycle** | Define → draft → test → refine → iterate → evaluate. Prompting is a loop. The correction to the source deck: testing is step 3, not an afterthought. |
| 11–16 min | **The taxonomy** | 11 task types as a working reference; the decision table from task type to technique. |
| 16–24 min | **Core craft** | System message, delimiters, zero-shot vs. few-shot. Before/after pairs, read aloud. |
| 24–30 min | **Chain-of-thought** | Why "think step by step" is a 2023 artefact; CoT as a dial on reasoning models; when reasoning does *not* help. |
| 30–38 min | **Structured output** | JSON that a pipeline can trust. Asking vs. guaranteeing. The incident-triage worked example. |
| 38–43 min | **The cost lever** | Cheap model + good prompt vs. expensive model + lazy prompt. Measured, with the equal-token-budget caveat. |
| 43–45 min | **Prompts as artifacts** | Version them, test them, put them in CI. Hand-off to Session 11. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. |

**Timing honesty:** this is a full 45 minutes with no slack. The structured-output segment is the one most likely to overrun because developers ask questions there. If you are behind at the 30-minute mark, compress the taxonomy segment to the decision table alone (the 11 types are in the reading) and protect structured output and the cost lever — those are the two segments that change what people build, not just what they type.

## Materials & tools

- Slides: `slides/outline.md`, built per `../powerpoint_instructions.md`.
- Self-study reading: `content/00-overview.md` → `content/99-key-takeaways.md`. The reading is the main deliverable; the deck is a 45-minute trailer for it.
- **Lab:** `exercises/lab.md` — a ~25-minute Colab notebook. Build a 6-case eval set from a release-notes task, run two prompt versions against it, score them, and get a table. Requires an API key (Anthropic or OpenAI); a no-key fallback path is included.
- Self-check: `exercises/quiz.md`. Discussion prompts: `exercises/discussion.md`.
- **Live demo (optional, no setup):** paste the before/after release-notes prompts from `content/05` into whatever chat tool the room uses and run them live. This lands harder than any slide.
- **Pre-reading (assign, do not embed):** Google's *Prompt Engineering* whitepaper (Boonstra) — still the best single linear narrative of the technique taxonomy, and **copyright-restricted, so it is reading only**. See `resources/sources.md`.

## Source & licence note

The **prompt-engineering cycle** and the **11-task-type taxonomy** come from Shaun Wassell's *ChatGPT Prompt Engineering Cookbook* (~Jan 2024) — a **LINK-ONLY** commercial deck. Both frameworks are re-authored here in our own words, re-drawn as our own diagrams, and materially extended: the source deck contains **zero verbatim prompts** (a fatal gap for something called a "Cookbook") and predates the entire chain-of-thought vocabulary. Every prompt in this session is written for this course.

Slides and content are built from **SLIDE-SAFE** sources: the **OpenAI Cookbook** and its GPT-5.x prompting guides (**MIT**), **DAIR.AI promptingguide.ai** (**MIT**, and our vendor-neutrality anchor), and **The Prompt Report v6** (**CC BY 4.0** — the authority for the technique glossary and the 58-technique taxonomy).

**Do not base slides on:** Google's prompting whitepapers (© Google, no reuse licence — assign as pre-reading), Anthropic's interactive prompt tutorial (Claude-3-era; its chain-of-thought chapter teaches an approach that reasoning models made obsolete — we reuse its *structure* only), or vendor blog prose. Full verdicts in `resources/sources.md`.

**Currency warning:** every model name, price, context-window size, and feature claim in this session is marked **"verify at delivery."** This material ages faster than any other session in the series. The *techniques* are durable; the *specifics* are not.
