# Sources — Session 10

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / MIT / BSD / public-domain. May derive slide and content text/figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NC / ND / vendor-proprietary / internal. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY.

**Session-specific note.** Every **verbatim prompt** in this session's `content/` and `exercises/` files was **authored for this course**. No prompt, example, code snippet, or table was copied from any source. This is deliberate: the primary source deck for this session contains zero verbatim prompts, so there was nothing to copy even had we wanted to. The prompts carry no third-party licence constraint.

---

## Slide-safe (embeddable with attribution)

**#2 — OpenAI Cookbook, including the GPT-5.x prompting guides.**
- URL: `https://developers.openai.com/cookbook` (note: `cookbook.openai.com` now redirects here — update old links). Repo: `https://github.com/openai/openai-cookbook`
- Licence: **MIT** — derive slides and content freely with attribution.
- Used for: the structured-output mechanism description (`content/06`, slide 17); the eval-driven posture that runs through `content/01` and `content/08` — *measured, eval-driven iteration over speculative prompt changes*; the scope-discipline framing ("implement exactly and only what is requested") behind the output-contract material in `content/05`.
- **Reuse verdict: SLIDE-SAFE.** On-slide attribution: "OpenAI Cookbook, MIT."
- *Verify at delivery:* which model-specific guide is current; the API surface for `response_format` / `strict`.

**#3 — DAIR.AI, *Prompt Engineering Guide* (promptingguide.ai).**
- URL: `https://www.promptingguide.ai/` · Repo: `https://github.com/dair-ai/Prompt-Engineering-Guide`
- Licence: **MIT** — derive freely.
- Used for: the vendor-neutral technique coverage underpinning `content/03`, `content/04`, and `content/05`; the model-agnostic framing throughout. This is the session's **neutrality anchor** — where a technique is described without vendor-specific language, this is why.
- **Reuse verdict: SLIDE-SAFE.** Attribute as "DAIR.AI Prompt Engineering Guide, MIT."
- *Note:* community-maintained, so depth is uneven chapter to chapter. Cross-check anything load-bearing against #4.

**#4 — Schulhoff, Ilie, Balepur et al. (2025), *The Prompt Report: A Systematic Survey of Prompt Engineering Techniques*, v6 (2025-02-26).**
- URL: `https://arxiv.org/abs/2406.06608`
- Licence: **CC BY 4.0** — derive freely with attribution.
- Used for: the **vocabulary**. Zero-shot / one-shot / few-shot / exemplar definitions in `content/03` and slide 9; the framing of the 11-type taxonomy as *practical* rather than systematic in `content/02`. This is the authority to cite when two engineers disagree about what a term includes — it defines 33 terms and catalogues 58 text-prompting techniques.
- **Reuse verdict: SLIDE-SAFE.** On-slide attribution: "Terminology: The Prompt Report v6, CC BY 4.0."
- *Limitations to state honestly:* ~17 months old at time of writing, predates the normalisation of reasoning models, and is **descriptive, not prescriptive** — it catalogues techniques, it does not tell you which to use.

**#8 — promptfoo (and the open-source eval tooling landscape).**
- URL: `https://www.promptfoo.dev/` · Repo licence: **MIT**. Also referenced: DeepEval, Ragas, Arize Phoenix (all open source).
- Used for: the tooling table in `content/08`.
- **Reuse verdict: SLIDE-SAFE** for the tool names, licences, and capabilities.
- ⚠️ **Governance flag, stated in the material:** promptfoo was **acquired by OpenAI (announced 2026-03-09)**, with a stated commitment to remain open source under its current licence and model-agnostic. For a multi-vendor organisation this is a **question to raise, not a settled fact**. The session presents it as such rather than asserting neutrality. *Verify current ownership, licence and commitments at delivery.*
- ⚠️ **Read "best eval tools" comparison articles adversarially** — much of that genre is written by vendors ranking their own competitors. The trustworthy signals are the licence file and repository activity, not the blog post.

---

## Link-only (reference / assign / paraphrase — never embed)

**#1 — Wassell, S., *ChatGPT Prompt Engineering Cookbook* (~January 2024, live webinar deck, 33 slides).**
- Licence: all-rights-reserved (commercial live-training material). **LINK-ONLY.**
- Used for: the **prompt-engineering cycle** (`content/01`) and the **11-task-type taxonomy** (`content/02`). Both frameworks are **re-authored in our own words, re-drawn as our own diagrams, and materially extended**; nothing is reproduced. The specific extensions: the cycle gains the corrected emphasis (testing as precondition, not step 3), a worked six-iteration example, a stopping-rule table, and a code harness; the taxonomy gains our four-way grouping, audience-specific examples, and the task→technique decision table, none of which are in the source.
- **Source defects carried as corrections, not repeated** (per `../../../AI_input.md` §6, defect #12): the deck's agenda promises "Advanced Prompt Engineering"; the delivered section is titled "Environment Improvements" and is ~75% unbuilt, with the **system message** named as a topic and never given a content slide. This session delivers that missing material in `content/05`.
- **The deck's fatal gap, named explicitly in `content/00`:** for something titled a "Cookbook" it contains **not one verbatim prompt** — only recipe *categories*. Every prompt in this session exists to fill that gap.
- **Vintage warning taught as a lesson:** the deck predates the entire chain-of-thought vocabulary (no zero-shot, few-shot, CoT, delimiters, output schemas, temperature, context window, hallucination, or prompt injection). We use its expiry as the argument for teaching the *testing loop* rather than a list of phrases.

**#5 — Anthropic — prompting best practices (Claude documentation) and related engineering posts.**
- URL: `https://platform.claude.com/docs/` (prompt-engineering section)
- Licence: proprietary vendor documentation, no open licence. **LINK-ONLY — re-teach in our own words; do not lift text or figures.**
- Used for (paraphrased, never quoted): the framing that prompt engineering **presupposes success criteria and a way to test against them** (`content/01`, slide 5); the practical on-ramp that **20–50 tasks drawn from real failures is a strong start** (`content/01`, `content/08`); the structured-output timeline in `content/06` and slide 17 (constrained decoding shipped by one vendor in Aug 2024, by the other in Nov 2025).
- **Currency:** the most current prompting reference available, and the correct place to check model-specific behaviour. *Verify all model names, parameter names, and feature claims here at delivery.*

**#6 — Anthropic — *Interactive Prompt Engineering Tutorial* (`github.com/anthropics/prompt-eng-interactive-tutorial`).**
- Licence: **unverified** ("view license" on the repo; not confirmed MIT). **LINK-ONLY — do not derive content.**
- **Used for STRUCTURE ONLY.** Its nine-chapter progression (basic structure → clarity → roles → separating data from instructions → formatting → examples → hallucinations → complex prompts) is a genuinely good course skeleton, and this session's ordering echoes it. **No content is taken.**
- 🔴 **Explicit warning carried into the material:** it is built on a Claude-3-era model generation, and **its chapter on step-by-step thinking teaches chain-of-thought as a prompt string** — the approach reasoning models made obsolete. `content/04` and slide 12 exist partly to correct exactly this. Do not assign it as reading without that caveat.

**#7 — Vendor engineering posts on multi-agent systems, context engineering, and prompt caching.**
- Representative: Anthropic's multi-agent research-system post and context-engineering post; Cognition's *Don't Build Multi-Agents* and its 2026 follow-up; provider caching documentation.
- Licence: proprietary vendor blogs. **LINK-ONLY — paraphrase findings, reproduce no text, figures, or numbers-as-graphics.**
- Used for (paraphrased): the **equal-token-budget caveat** in `content/07` and slide 20 — the finding, reported by the vendor itself, that **token usage alone explained roughly 80% of performance variance** in a multi-agent comparison consuming ~15× the tokens; the cache-hit-rate-as-engineering-outcome argument in `content/07`.
- ⚠️ The cache-hit-rate case study (single-digit → 80%+ by prompt reordering, with total spend more than halved) is **vendor-adjacent and is presented in the material as illustrative, not as a verified benchmark.** Keep that framing.

**#9 — Neutral research on agent/multi-agent scaling.**
- Representative: *Towards a Science of Scaling Agent Systems* (`https://arxiv.org/abs/2512.08296`); UC Berkeley's *Measuring Agents in Production* (2025-12).
- Licence: arXiv preprints — assume all-rights-reserved unless a CC notice is displayed. **LINK-ONLY** unless you verify a CC licence per paper.
- Used for: the counterweight in `content/07` — that holding total thinking-token budget equal largely dissolves the reported architectural advantage. This is what makes the equal-token-budget question defensible rather than merely skeptical.
- *Verify at delivery:* current version and whether a CC licence applies if you want to quote.

**#10 — Boonstra, L., *Prompt Engineering* whitepaper (Google), v4 (Feb 2025), ~65–69 pp.**
- URL: `https://www.kaggle.com/whitepaper-prompt-engineering`
- Licence: **© Google, no open reuse licence.** Distribution via Kaggle is not a reuse grant. **LINK-ONLY — cannot derive slides. Assign as pre-reading only.**
- Why it is still recommended reading: it remains the best single **linear narrative** of the technique taxonomy — zero/one/few-shot → system/role/contextual → step-back → CoT → self-consistency → Tree-of-Thought → ReAct → automatic prompt engineering. It maps closely onto this session's arc, which is why it makes an excellent pre-read and a legally impossible slide source.
- Weaknesses to flag when assigning: Vertex/Gemini-centric configuration examples, and it predates the reasoning-model shift — so its treatment of sampling parameters is dated on exactly the point `content/04` corrects.

**#11 — Weng, L., *Prompt Engineering* (Lil'Log, 2023-03-15).**
- URL: `https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/`
- Licence: personal blog, all-rights-reserved. **LINK-ONLY.**
- Used for: one framing, expressed in our own words — prompting as an **empirical science requiring experimentation, not a set of magic words**. That framing is the spine of `content/01`.
- *Currency:* **older than the source deck this session replaces.** Cite for lineage; do not teach the technique inventory from it — superseded by #4.

**#12 — Huyen, C., *AI Engineering: Building Applications with Foundation Models* (O'Reilly, 2025).**
- Licence: © O'Reilly. **LINK-ONLY** — buy copies. Companion repo `github.com/chiphuyen/aie-book` is free.
- Used for: general framing on evaluation of non-deterministic systems, behind `content/08`. Recommended as the vendor-neutral reference text for anyone going deeper.

---

## Further reading (LINK-ONLY, high quality — assign, don't slide)

| Topic | Suggestion | Why |
|---|---|---|
| The technique taxonomy as a narrative | Google *Prompt Engineering* whitepaper (#10) | The best single linear read on the technique landscape — and the reason it is pre-reading rather than slides is itself a teachable licensing lesson. |
| Current, model-specific practice | Anthropic prompting best practices (#5) | The living reference. Check it at delivery; it is where the model-specific behaviour actually lives. |
| The defensible vocabulary | The Prompt Report v6 (#4) | Read once, own the terms. The only slide-safe academic backbone here. |
| Vendor-neutral technique coverage | DAIR.AI guide (#3) | MIT-licensed, model-agnostic, and it tracked both the context-engineering and agent shifts. |
| Evaluation of non-deterministic systems | Huyen, *AI Engineering* (#12) | The strongest neutral treatment of the eval problem this session only opens. |
| ⚠️ **Read with a caveat** | Anthropic interactive tutorial (#6) | Excellent exercise *structure*, obsolete *content*. If assigned, say so up front and name the chain-of-thought chapter specifically. |

---

## Currency register for this session

This session ages faster than any other in the series. Before delivery, verify:

| Item | Where it appears | What to check |
|---|---|---|
| Model names and IDs | `content/03`, `04`, `06`, `07`; `exercises/lab.md`; slides 15–19 | All are `<placeholders>`. Resolve every one. |
| Token prices | `content/07`; `exercises/lab.md`; slide 19 | Both input and output, both tiers. The 20× ratio is the load-bearing claim, not the absolute numbers. |
| Reasoning parameter names | `content/04` | `thinking` / budget / effort parameter names and limits differ by vendor and change. |
| Structured-output API surface | `content/06` | `response_format` shape, `strict` flag, tool-schema fields. |
| Prompt-caching discounts and mechanics | `content/07` | Discount percentages, write premiums, TTLs, whether caching is explicit or automatic. |
| promptfoo ownership and licence | `content/08`; slide 23 | Acquired by OpenAI 2026-03-09; verify the open-source commitment still holds. |
| The Prompt Report version | `resources`, slide 9 | v6 at time of writing; check for a newer version. |

Everything else in this session — the cycle, the taxonomy, the mechanism of few-shot, the reason CoT works, the schema-design rules, the equal-token-budget discipline, and the prompts-as-artifacts practice — is durable. **Teach the durable parts as durable, and mark the volatile parts as volatile.** That distinction is itself one of the session's lessons.
