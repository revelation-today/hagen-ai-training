# Sources — Session 12: Agents and Tool Use

Every source used, with licence status and a one-line reuse verdict. **SLIDE-SAFE** = permissive / CC-BY / MIT / Apache / open standard — may derive slides and figures *with attribution*. **LINK-ONLY** = all-rights-reserved, proprietary, or no stated licence — assign as reading or run as a live demo, **never copy onto a slide.** Verdicts as of 2026-07-19; the currency register at the end lists what must be re-checked before delivery.

---

## 0. Provenance — where this session's content comes from

**This session exists because the corpus does not cover it.** The source-deck extract (`../../../AI_input.md`) is explicit: agents, ReAct, and tool use appear in **exactly one** of the seven source decks (the AGI deck, §2.5), at a conceptual level — the definition, ReAct, Plan & Execute, Reflection, and a mention of LangChain/LangGraph. The gap register (§4, item 5) names the missing half directly: *"Agentic patterns as engineering — the AGI deck covers ReAct/Plan-Execute/Reflection conceptually; nothing covers building, testing, or bounding them in production."*

So the split is:

- **The pattern vocabulary** (agent as semi-autonomous — autonomy, decision-making, adaptation; ReAct's Thought–Action–Observation; Plan & Execute; Reflection; deep research as the synthesis of all three) is **conceptually sourced from the AGI deck**, rebuilt in our own words and our own diagrams. **LINK-ONLY** — see #10.
- **Everything else in this folder is original work** written for this course: every code block, every trace, every table, every diagram, every worked example, every cost figure, and the entire workflow-vs-agent, when-not-to-build, and production treatment.

**Consequence for the deck-builder:** virtually everything on these slides is original course material and is **SLIDE-SAFE without external attribution.** The constraints below are about what may *not* be added.

---

## SLIDE-SAFE — build slides and figures from these (with attribution)

**1. Hugging Face — AI Agents Course, and `smolagents`.**
Course: https://huggingface.co/learn/agents-course · Org: https://huggingface.co/agents-course · Library: https://github.com/huggingface/smolagents
**`smolagents` is Apache-2.0 → SLIDE-SAFE with attribution.** Course units are Apache-2.0 — **verify per unit** before deriving from a specific one.
Why it is this session's recommended follow-on: it is **multi-framework rather than single-vendor** (covers `smolagents`, LlamaIndex, *and* LangGraph), free and certified, and its core is roughly a thousand readable lines with abstractions "kept to their minimal shape above raw code." For engineers who learn by reading implementations, it beats every PDF in this space — and a participant who has completed `exercises/lab.md` will recognise the loop immediately.
Two of its ideas are cited in `content/03` §6: **code agents** (the agent writes its actions as code rather than emitting tool-call dicts, reported to need fewer steps on hard benchmarks) and its sandboxing posture (**never execute model-written code unsandboxed** — a genuinely good security teaching moment that Session 14 picks up).
⚠️ Verify the current version and the per-unit licence at delivery.

**2. Model Context Protocol — specification, documentation, and SDKs.**
Agentic AI Foundation (Linux Foundation); originated at Anthropic and donated December 2025, with OpenAI and Block as co-founding members. https://modelcontextprotocol.io · https://github.com/modelcontextprotocol
**Open standard; SDKs open source (MIT/Apache — verify per repo) → SLIDE-SAFE with attribution.**
Established in Session 11 and **assumed, not re-taught, here.** This session relies on three of its properties: the model emits a call against a schema and never touches the system; the server is the enforcement point; tools act and resources are read.
⚠️ **The final specification publishes 2026-07-28.** If this session is delivered before that date, say so at slide 5.

**3. Anthropic Python SDK.** https://github.com/anthropics/anthropic-sdk-python — **MIT → SLIDE-SAFE** (the library).
Every Python example in `content/03`, `content/07`, and `exercises/lab.md` follows its documented Messages API tool-use patterns: the tool-definition shape, the `stop_reason == "tool_use"` loop condition, `tool_use_id` matching, and `is_error` on tool results. Note the distinction carefully: **the SDK is MIT and its usage patterns are freely derivable; the product documentation prose is not** (see #7).

**4. Arize Phoenix · other open-source tracing and evaluation tooling.**
https://github.com/Arize-ai/phoenix · plus DeepEval (https://github.com/confident-ai/deepeval) and promptfoo (https://github.com/promptfoo/promptfoo) as established in Sessions 10–11 — **open source (MIT/Apache — verify per repo) → SLIDE-SAFE.**
Cited in `content/07` §2 as the graduation path once print-statement tracing is outgrown. The tracing *principle* in that file is ours; the tools are named so the recommendation is not single-vendor.
⚠️ **Governance disclosure carried forward from Session 11:** promptfoo was acquired by OpenAI (announced 2026-03-09), with stated commitments to remain open source and model-agnostic. For a multi-vendor organisation that is a procurement question, not a settled fact. State it plainly.

**5. Agent benchmarks — τ²-bench, SWE-bench Verified, GAIA, OSWorld, WebArena, Terminal-Bench.**
Representative: https://github.com/sierra-research/tau2-bench · https://benchmarkingagents.com/osworld/
**Individual licences vary — verify before deriving from any specific one.** In this session they are **named, not derived from**: `content/06` and slide 17 discuss the *methodology critique* (cost-blindness, harness sensitivity, pass^k collapse, contamination), which is commentary about them rather than reuse of them. τ²-bench is worth naming aloud for this audience because it includes a **telecom** domain and tests an agent's ability to use API tools while following a company policy — the closest public analogue to the work in this room.

---

## LINK-ONLY — reference, assign, or paraphrase; never copy onto a slide

**6. Anthropic — "Building Effective Agents"** (2024-12-19). https://www.anthropic.com/research/building-effective-agents
Supporting engineering posts: *Effective context engineering for AI agents* (2025-09-29), *Demystifying evals for AI agents* (~2026-01), *Writing effective tools for AI agents*, *How we built our multi-agent research system* (2025-06). Extended material at https://resources.anthropic.com/building-effective-ai-agents
**Proprietary, no open licence → LINK-ONLY.**
The canonical text for Part B of the agent literature, and the conceptual spine of `content/02`. Three things are re-expressed here **in our own words, attributed by concept and never quoted**:
- The **workflows-vs-agents distinction** — workflows orchestrate through predefined code paths; agents dynamically direct their own processes (`content/02` §1, slide 6).
- The **composition-pattern taxonomy** — prompt chaining, routing, parallelisation, orchestrator-workers, evaluator-optimiser, autonomous agents (`content/02` §5, redrawn as our own Mermaid).
- The **anti-hype posture** — most things you want are workflows; start simple; add agency only when flexibility outweighs latency, cost, and compounding error.
The multi-agent post supplies the three data points in `content/06`: the **~90% improvement** claim, the **~80%-of-variance-is-token-usage** finding, and the **~15× token consumption**. **These are facts and may be stated; the prose and figures may not be reproduced.**
**Excellent assigned pre-reading.** A vendor telling you to buy less of its product is a credibility signal — say so in the room.

**7. Anthropic — Claude documentation** (tool use, agent design, structured outputs). https://platform.claude.com/docs — **Proprietary → LINK-ONLY.** The living reference a presenter must check before delivering the code slides. Concepts re-expressed throughout; no text or figure reproduced. *(The SDK itself is MIT — see #3.)*

**8. Cognition — "Don't Build Multi-Agents"** (2025-06-12, Walden Yan). https://cognition.com/blog/dont-build-multi-agents
**And its 2026 follow-up**, shipping a coordinator that assigns work to managed sub-instances in isolated environments: https://cognition.com/blog/multi-agents-working
**Proprietary → LINK-ONLY.** The opposing position in `content/06` §2, and the reversal. Two principles are paraphrased: **share context thoroughly** (full traces, not individual messages), and **actions carry implicit decisions, and conflicting decisions produce bad results.** The 2026 post does not retract the earlier essay by name; the architectural concession is unambiguous, and that is stated as our observation, not theirs.

**9. Neutral research on agent scaling and production reliability.**
Representative: *Towards a Science of Scaling Agent Systems* (https://arxiv.org/abs/2512.08296) · UC Berkeley, *Measuring Agents in Production* (2025-12) · related DeepMind work on multi-agent coordination overhead.
**Licences vary by venue — verify before reproducing any figure; the findings themselves are cited as fact.**
The reality check in `content/06` §3: multi-agent systems frequently underperform single agents because of coordination overhead; adding agents or compute often degrades performance; and **at equal thinking-token budget the architectural advantage largely evaporates.** This is the material with no product to sell, and it should be presented as such.
⚠️ Verify version and findings at delivery — this is the fastest-moving literature in the session.

**10. Ozdemir — *AGI Demystified*** (O'Reilly, ~2026), corpus source #7, §AI Agents. **Commercial, all-rights-reserved → LINK-ONLY.**
The corpus's only agent material and the conceptual origin of the pattern vocabulary in `content/01` and `content/04`: the semi-autonomous definition (autonomy + decision-making + adaptation), ReAct's Thought–Action–Observation loop, Plan & Execute (a large model plans, small fast models execute), Reflection (a critique module before the final answer), and deep research as the synthesis of all three. **Ideas rebuilt in our own words, examples, diagrams, and code; nothing reproduced.** See `../../../AI_input.md` §2.5.
Note the deck's own defining virtue, worth borrowing: it leads with negative results. This session tries to do the same.

**11. Nield — *LLM System Safety and Security*** (O'Reilly, ~late 2023), corpus source #4. **Commercial → LINK-ONLY.**
Supplies exactly one thing, and it is the hand-off in `content/07` §7 and slide 21: the hazard-triangle **initiating mechanism** *"an API that directly acts on LLM output."* One line, written before agents were common, and precisely right. **Framing paraphrased and attributed; the deck itself is Session 14's material.**
Correction carried forward from Session 14's provenance note: that deck is titled *Safety and Security* but contains **zero** adversarial-security content. Do not present it as covering agent attack surface.

**12. OpenAI — "A Practical Guide to Building Agents"** (PDF, ~2025). https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf — **Proprietary → LINK-ONLY.**
Its **guardrails** chapter is the best short treatment of bounding agent behaviour and is worth assigning alongside `content/07`. ⚠️ Note the provenance honestly in the room: it is filed under `/business/guides-and-resources/`, not engineering, and it is **the most marketing-flavoured of the major agent documents.** Its own SDK documentation is more current than the PDF. Useful as a contrast in tone with #6.

**13. LangChain / LangGraph — documentation and academy.** https://docs.langchain.com · https://academy.langchain.com
**The libraries are MIT (derivable); the documentation and course prose are LINK-ONLY.**
Named in `content/03` §6 as one framework among several, for its distinctive idea — agents as an explicit state machine of State, Nodes, and Edges, which makes control flow reviewable and pushes you usefully back toward workflows. ⚠️ **State the lock-in caveat:** the academy is free because it sells the observability product, and teaching LangGraph teaches LangGraph, not agents. Recommend the concepts framework-neutrally.

**14. OpenAI Agents SDK.** Named in `content/03` §6 for its contrasting philosophy — code-first orchestration with no graph declared up front, hand-offs modelled as tools. **Library licence varies; documentation prose LINK-ONLY.** Mentioned as one of three genuinely disagreeing design positions, not recommended.

**15. Chip Huyen — *AI Engineering: Building Applications with Foundation Models*** (O'Reilly, 2025). Companion repo (free): https://github.com/chiphuyen/aie-book — **© O'Reilly → LINK-ONLY.**
The most vendor-neutral systematic treatment in book form, and the neutrality anchor for this half of the course. Its agent chapter (plan generation → execution → reflection and error correction) and its evaluation chapter are the natural next step past `content/04` and `content/07`. One idea paraphrased in `content/01` §5: **agents typically require more capable models because mistakes compound over multi-step tasks.** Buy copies; do not reproduce.

**16. Lilian Weng — "LLM Powered Autonomous Agents"** (2023-06-23). https://lilianweng.github.io/posts/2023-06-23-agent/ — **Personal blog, all rights reserved → LINK-ONLY.**
The historically important agent-taxonomy post. **Cite for lineage; do not teach from it** — it predates the current generation of models, tool APIs, and MCP entirely. Listed here so nobody rediscovers it and mistakes it for current.

---

## Corrections and clarifications this session makes

| Claim in circulation | Correction made here | Where |
|---|---|---|
| "Agent" = anything with an LLM in it | An agent requires autonomy **and** decision-making **and** adaptation. Most products described as agents are workflows | `content/01`, slide 7 |
| More agents / more compute → better results | Neutral research finds the opposite is common: coordination overhead frequently degrades performance | `content/06` §3 |
| Multi-agent beats single-agent (headline reading) | The same write-up reports token usage explained ~80% of the variance at ~15× the tokens. At equal budget the advantage largely evaporates | `content/06` §2–3 |
| A benchmark score is a reliability estimate | pass^k commonly runs 15–25 points below pass^1. **A 90% benchmark score ≈ 70% production reliability** | `content/06` §5, `content/07` §4 |
| Agent quality is a single number | Cost per task belongs in the same table. Essentially no major agent benchmark scores it | `content/04` §4, slide 19 |
| "Human in the loop" is a control | Necessary and not sufficient — the human must be *qualified* and the output must be *checkable*. This gets harder as the agent improves | `content/07` §6 |
| Prompt-level constraints bound an agent | Least privilege belongs in code and in the service identity, never in a system prompt | `content/07` §1 |
| The "Thought" shows the model's reasoning | It is generated text about the process, not a log of it | `content/03` §2 |

---

## ⚠️ Currency register — verify before delivery

| Item | Status at authoring (2026-07-19) | Action before delivery |
|---|---|---|
| **MCP final specification** | Publishes **2026-07-28** | If delivering earlier, flag slide 5 as pre-final-spec. Session 11 carries the full constraint |
| **Model IDs in all code** | Placeholder `claude-opus-4-8`, set in one constant per file | **Verify current IDs.** They are isolated for exactly this reason |
| **SDK parameter shapes** (tool definitions, `stop_reason`, thinking/effort parameters) | Follows current Messages API patterns | Verify against current SDK documentation — this surface has changed more than once |
| **Per-token prices** (all cost figures in `content/04` §1, §4 and slide 19) | Illustrative ratios only, marked as such | **Recompute.** Slides 12 and 19 must carry the "illustrative" footer |
| **`smolagents` version and per-unit course licence** | Apache-2.0, actively maintained | Verify version and confirm the licence of any specific course unit you derive from |
| **Multi-agent literature** | Two vendor positions, one reversal, equal-token-budget preprints | **Re-check.** This is the fastest-moving material in the session. If the picture has changed, that change *is* the lesson — teach it |
| **pass^k gap and benchmark cost-blindness** | 15–25 points; essentially no major suite scores cost | Re-verify; benchmark methodology is improving and these figures may soften |
| **promptfoo ownership** | MIT; OpenAI acquisition announced 2026-03-09 | Verify the licence is unchanged; disclose ownership either way |
| **Framework landscape** (LangGraph, OpenAI Agents SDK, `smolagents`) | Three genuinely disagreeing design positions | Verify none has been deprecated or absorbed. The disagreement is the teaching point, not the roster |

---

## Further reading (LINK-ONLY, high quality)

Assign these; do not build slides from them.

1. **Anthropic — *Building Effective Agents*** (#6). The single best hour a participant can spend after this session. Read it for the six patterns and for the anti-hype posture.
2. **Anthropic — *How we built our multi-agent research system*** (#6). Read it specifically to find the paragraph reporting that token usage explained ~80% of the variance. **Finding that paragraph yourself is the exercise** — it is the best single reading assignment in the course.
3. **Cognition — *Don't Build Multi-Agents*** and its 2026 follow-up (#8). Read both, in order, and notice that the second one does not mention the first.
4. **Anthropic — *Demystifying evals for AI agents*** (#6). Why agent evaluation differs from chat evaluation: agents act over many steps, mutate external state, and can "win" in ways a rigid benchmark scores as failure. Its on-ramp — **20–50 tasks from real failures** — is the antidote to eval paralysis and it underpins `content/07` §4.
5. **Chip Huyen — *AI Engineering***, agent and evaluation chapters (#15). The neutral, systematic treatment.
6. **Hugging Face AI Agents Course + `smolagents` source** (#1, also SLIDE-SAFE). For anyone who did the lab: read the thousand lines. You will recognise all of it.
7. **OpenAI — *A Practical Guide to Building Agents***, guardrails chapter (#12). Read the guardrails chapter; read the rest with the funnel in mind.

> **A standing warning on secondary sources for this topic.** Search results for "agents", "multi-agent", and "MCP" are heavily polluted by content marketing and AI-generated filler — including comparison listicles written by vendors ranking their own competitors. **Prefer primary sources: the specification, the repository, the licence file, and the paper.** Then teach the habit itself: before believing a comparison, check who wrote it and what they sell. That habit is more durable than anything else in this session.
