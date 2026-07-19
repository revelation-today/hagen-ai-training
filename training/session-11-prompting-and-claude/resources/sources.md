# Sources — Session 11: Prompting II + Working With Claude

Every source used, with licence status and a one-line reuse verdict. **SLIDE-SAFE** = permissive / CC-BY / MIT / Apache / open standard — may derive slides and figures *with attribution*. **LINK-ONLY** = all-rights-reserved, proprietary, or no stated licence — assign as reading or run as a live demo, **never copy onto a slide.** Verdicts as of 2026-07-19; the currency register at the end lists what must be re-checked before delivery.

---

## 0. Provenance — where this session's content comes from

This session is unusual in the series: **most of it has no source at all.**

- **Part A (Prompting II)** is original work. Every prompt, worked example, model output, failure taxonomy, test case, and diagram in `content/01`–`03` and the slide deck was written for this course. The corpus's only prompt-engineering material (Wassell, *ChatGPT Prompt Engineering Cookbook*, ~Jan 2024 — see `../../../AI_input.md` §2.3) contains **zero verbatim prompts**, and its vocabulary predates chain-of-thought, structured outputs, and evaluation entirely. It contributed nothing here beyond the observation that the gap existed.
- **Part B (Working with Claude)** is **fully authored**. There is no corpus source and no reuse-safe third-party source for it. It rests on general knowledge of the product surface plus the durable workflow principles the rest of the course establishes.

**Consequence for the deck-builder:** virtually everything on these slides is original course material and is **SLIDE-SAFE without external attribution.** The constraints below are about what may *not* be added.

---

## SLIDE-SAFE — build slides and figures from these (with attribution)

**1. Model Context Protocol — specification, documentation, and SDKs.**
Agentic AI Foundation (Linux Foundation); originated at Anthropic and donated December 2025, with OpenAI and Block as co-founding members. https://modelcontextprotocol.io · spec and SDKs: https://github.com/modelcontextprotocol · blog: https://blog.modelcontextprotocol.io
**Open standard; SDKs are open source (MIT/Apache — verify per repo) → SLIDE-SAFE with attribution.** Source for `content/07` and slide 19: host/client/server architecture, stdio and Streamable HTTP transports (HTTP+SSE deprecated), the stateless protocol core, tools vs. resources vs. prompts, and freshness metadata on list/read results.
⚠️ **The final specification publishes 2026-07-28.** Verify all specifics — including the exact names of freshness/scope fields — against the published final spec. **Land this session after that date.**

**2. OpenAI Cookbook — prompting guides (GPT-5.x series).**
https://developers.openai.com/cookbook · repo: https://github.com/openai/openai-cookbook — **Licence: MIT → SLIDE-SAFE.** Established as a spine source in Session 10. Not directly drawn on here, but its central posture — *measured, eval-driven iteration over speculative prompt changes* — is the professional stance `content/03` builds on, and it is the reuse-safe place to point developers who want a second, non-Anthropic treatment of the same discipline.

**3. DAIR.AI — Prompt Engineering Guide (promptingguide.ai).**
Elvis Saravia / DAIR.AI. https://www.promptingguide.ai · repo: https://github.com/dair-ai/Prompt-Engineering-Guide — **Licence: MIT → SLIDE-SAFE.** The vendor-neutral reference for the technique vocabulary this session assumes from Session 10. Now covers context engineering and agents as well as prompting. **The neutrality anchor** — cite it whenever the room asks whether this is Claude-specific advice.

**4. The Prompt Report: A Systematic Survey of Prompting Techniques.**
Schulhoff, Ilie, Balepur et al. (31 authors), arXiv:2406.06608, v6 2025-02-26. https://arxiv.org/abs/2406.06608 — **Licence: CC BY 4.0 → SLIDE-SAFE with attribution.** The defensible, non-vendor definitional authority for the 33-term vocabulary and the ~58-technique taxonomy. Use it to settle terminology disputes ("what do we mean by few-shot here?"). Descriptive rather than prescriptive — it will not tell anyone what to do, which is what this session supplies.

**5. promptfoo.** https://www.promptfoo.dev · repo: https://github.com/promptfoo/promptfoo — **Licence: MIT → SLIDE-SAFE.** The lowest-barrier production-grade prompt-evaluation tool: CLI, YAML configs, CI/CD integration, and red-teaming checks. The minimal runner in `content/03` and `exercises/lab.md` is deliberately hand-rolled so participants understand the mechanism; **promptfoo is the correct graduation path** once a team outgrows ten cases.
⚠️ **Governance disclosure to make in the room:** promptfoo was **acquired by OpenAI (announced 2026-03-09)**, with stated commitments to remain open source under its current licence and model-agnostic. For a multi-vendor organisation this is a procurement question, not a settled fact. State it plainly and let the team decide; do not present the tool as neutral without the caveat.

**6. DeepEval · Ragas · Arize Phoenix.** https://github.com/confident-ai/deepeval · https://github.com/explodinggradients/ragas · https://github.com/Arize-ai/phoenix — **Open source (MIT/Apache — verify per repo) → SLIDE-SAFE.** Alternatives to promptfoo without the ownership caveat: DeepEval for broad metric coverage, Ragas for RAG-specific evaluation only, Phoenix for tracing and observability. Listed on the resources slide so the recommendation is not single-tool.

**7. Anthropic Python SDK.** https://github.com/anthropics/anthropic-sdk-python — **Licence: MIT → SLIDE-SAFE** (the library). Every Python example in `content/01`, `03`, `06` and the lab follows its documented Messages API patterns. Note the distinction carefully: **the SDK is MIT and its usage patterns are freely derivable; the product documentation prose is not** (see LINK-ONLY #10).

---

## LINK-ONLY — reference, assign, or demo live; never copy onto a slide

**8. Anthropic — Claude documentation** (prompting best practices, extended thinking, tool use, structured outputs, Projects, Artifacts). https://platform.claude.com/docs — **Proprietary, no open licence → LINK-ONLY.** The living reference for everything in Part B, and **the thing a presenter must check before delivering it.** Concepts are re-expressed in our own words throughout `content/04`–`06`; no text or figure is reproduced.

**9. Anthropic — engineering blog.** Notably *Building Effective Agents* (2024-12-19, https://www.anthropic.com/research/building-effective-agents), *Effective context engineering for AI agents* (2025-09-29, https://www.anthropic.com/engineering/effective-context-engineering-for-agents), *Demystifying evals for AI agents* (~2026-01, https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents), *Writing effective tools for AI agents* — **Proprietary → LINK-ONLY.**
Two ideas re-expressed in this session, both attributed by concept rather than quotation: the "20–50 tasks drawn from real failures is a good start" on-ramp that defeats eval paralysis (`content/03`), and the framing of context as something deliberately curated rather than accumulated (`content/05`). **Excellent assigned reading. Never lift text or figures.**

**10. Claude product interface** — chat, Projects, Artifacts, extended thinking, connectors. **Proprietary product UI → LIVE DEMO ONLY.** May be demonstrated live (slide 17). **Never screenshot into the deck.** If a no-network fallback is required, build a hand-drawn schematic, not a captured screen.

**11. Google — *Prompt Engineering* whitepaper** (Lee Boonstra). https://www.kaggle.com/whitepaper-prompt-engineering — **© Google, no open licence → LINK-ONLY.** The best-organised linear narrative of the technique taxonomy, and unusable as a slide source. Also materially dated: it predates the reasoning-model shift, which is precisely the correction `content/06` makes. Assign as optional pre-reading with that caveat attached.

**12. Anthropic — Interactive Prompt Engineering Tutorial.** https://github.com/anthropics/prompt-eng-interactive-tutorial — **Licence unverified → treat as LINK-ONLY.** 🔴 **Do not build on its content.** It is built on a model generation several revisions obsolete, and its chapter on step-by-step thinking teaches chain-of-thought-as-prompt-string, which `content/06` explicitly corrects as an expired technique. Its *exercise structure* is good; its material is not. Flagged here so nobody rediscovers it and mistakes it for current.

**13. Chip Huyen — *AI Engineering: Building Applications with Foundation Models*.** O'Reilly, 2025. Companion repo (free): https://github.com/chiphuyen/aie-book — **© O'Reilly → LINK-ONLY.** The most vendor-neutral systematic treatment in book form; its evaluation chapter is the natural next step for anyone who wants more than `content/03` offers. Buy copies; do not reproduce.

**14. Wassell — *ChatGPT Prompt Engineering Cookbook*** (~Jan 2024, webinar deck, corpus source #5). **Commercial, all-rights-reserved → LINK-ONLY.** Listed for provenance honesty: it is the corpus's only prompting source and it contributed **nothing** to this session. It contains no verbatim prompts, its Section 5 is largely unbuilt, and its vocabulary predates chain-of-thought, structured outputs, delimiters, temperature, and evaluation. See `../../../AI_input.md` §2.3 and §4 item 2.

---

## Corrections this session makes to earlier or source material

| Claim in older material | Correction made here | Where |
|---|---|---|
| "Add 'let's think step by step' as a core technique" | Productised into a reasoning budget on reasoning models. Redundant as a prompt string; spend the space on context. Steer the *shape* of reasoning by asking for an inspectable intermediate instead | `content/06` |
| "Prompt engineering is phrasing" | Phrasing is necessary and no longer sufficient. Success criteria and a way to test against them come first — without them it is guessing, not engineering | `content/03` |
| "Better prompts need better models" | Frequently the reverse: a better prompt on a cheaper model beats a worse prompt on an expensive one. Measurable, and commonly surprising | `content/03`, slide 15 |
| MCP as a vendor-specific hook | Foundation-governed multi-vendor standard since Dec 2025; stateless at the protocol layer; HTTP+SSE transport deprecated | `content/07` |
| "Asking for JSON gives you JSON" | Instruction-following is a request; a schema-constrained call is a contract. Not the same guarantee | `content/01` |

---

## ⚠️ Currency register — verify before delivery

**This session ages faster than any other in the series.** Check every row.

| Item | Status at authoring (2026-07-19) | Action before delivery |
|---|---|---|
| **MCP final specification** | Publishes **2026-07-28** | **Land the session after this date.** Verify transports, freshness/scope field names, and the tools/resources model against the final spec |
| Claude model IDs (all code examples) | Placeholders `claude-sonnet-4-5`, `claude-haiku-4-5` | **Verify current IDs.** They are set in one constant per file for exactly this reason |
| Extended-thinking parameter shape and budget units | `thinking={"type": "enabled", "budget_tokens": N}` | Verify — this API has changed more than once |
| Structured-output / tool-schema mechanism and any strictness flag | Tool use with forced `tool_choice` | Verify current mechanism and guarantees |
| Per-token pricing (all cost figures) | Illustrative arithmetic only, marked as such | Recompute. Slides 15 and 18 must carry the "illustrative" footer |
| Projects / Artifacts feature names, limits, availability | As described in `content/05` | Verify names, size limits, and availability for your organisation's plan |
| Connector availability and organisational policy | Unknown per-org | Check what is actually enabled and permitted **before** demoing it |
| promptfoo ownership and licence | MIT; OpenAI acquisition announced 2026-03-09 | Verify the licence is unchanged; disclose ownership in the room either way |
| Data-handling policy (`content/08`) | Generic guidance only | **Replace with your organisation's current policy.** This is the one section a presenter must not improvise |

---

## Further reading (LINK-ONLY, high quality)

Assign these; do not build slides from them.

1. **Anthropic — Claude documentation, prompting best practices.** https://platform.claude.com/docs — the current-model reality check, and the most useful hour a participant can spend after this session.
2. **Anthropic — *Demystifying evals for AI agents*.** The vendor-side argument for the same discipline `content/03` teaches, with the "20–50 tasks from real failures" on-ramp.
3. **Anthropic — *Effective context engineering for AI agents*.** The post that named the shift from "how do I phrase this?" to "what belongs in the context window, and what do I deliberately keep out?"
4. **Chip Huyen — *AI Engineering*, evaluation chapter.** The neutral, systematic treatment of evaluating non-deterministic systems where the right answer is not obvious.
5. **The Prompt Report v6** (also SLIDE-SAFE, #4). Read for the vocabulary; it is the reference that settles arguments about what a term means.
6. **MCP specification** (also SLIDE-SAFE, #1). Short, readable, and the primary source — far better than any secondary "MCP guide" blog, most of which are low-quality and SEO-driven.

> **A note on secondary sources for this topic.** Search results for prompting, evals, and MCP are heavily polluted by content marketing and AI-generated filler — including "best eval tools" listicles written by vendors ranking their own competitors. Prefer primary sources: the specification, the repository, the licence file. Point participants at those, and teach the habit of checking who wrote a comparison before believing it.
