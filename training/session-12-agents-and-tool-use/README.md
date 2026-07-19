# Session 12 — Agents and Tool Use

**Block:** Application · **Format:** 45 min content + 15 min Q&A · **Hands-on:** yes — a short Python lab (`exercises/lab.md`)

---

## One-paragraph summary

Six other sessions in this series mention agents. None of them teaches what one *is*. This session closes that gap. An agent is not a smarter model and it is not a better prompt — it is **a language model wired into a loop and given the ability to act**: it decides what to do next, calls a tool, reads the result, and decides again, until it judges the task done. That one architectural change is the whole topic. It buys you the ability to handle tasks whose steps you cannot enumerate in advance; it costs you determinism, predictability, an order of magnitude in tokens, and a much larger blast radius when things go wrong. So the centre of this session is not "how to build an agent" but **"should you"** — and the honest answer, from the vendors who sell agents as loudly as from the researchers who measure them, is usually *no, build a workflow*. We teach the workflow-versus-agent distinction as the primary decision, the ReAct Thought → Action → Observation loop as the core mechanism (built by hand in Python, with the loop visible rather than hidden inside a framework), Plan & Execute and Reflection as the two variations worth knowing, and the multi-agent literature as a case study in reading vendor claims — because two major labs published flatly contradictory advice, one of them quietly reversed, and the neutral research undercuts both. We close on the one sentence that hands over to Session 14: an agent is precisely *an API acting on model output*, which is the single hazard the safety framework this course inherited names by name.

## Audience & level

Qualcomm release / problem / configuration managers and developers. Everyone will have heard "agent" used in three incompatible ways this quarter; the first ten minutes fix the vocabulary. The Python is for developers and is fully explained line-by-line in `content/03`; the decision content — workflows vs. agents, when not to build one, how to read a vendor's agent benchmark — needs no code at all and is the part managers will use first. Non-coders should follow the reading track in the lab rather than the notebook.

## Learning objectives

By the end, a participant can:

- **Define** an agent precisely — an LLM in a loop with tools, exhibiting autonomy, decision-making, and adaptation — and distinguish it from a plain prompt, a tool call, and a fixed workflow.
- **Decide** between a workflow and an agent for a given task using a stated criterion (are the steps enumerable in advance?), and justify the choice out loud.
- **Trace** a ReAct Thought → Action → Observation loop step by step, and **read** the ~40 lines of Python that implement one without a framework.
- **Explain** when Plan & Execute and Reflection pay for themselves, and what each one costs.
- **Argue** the case *against* building an agent for at least three concrete task types where a deterministic script is the correct answer.
- **Interrogate** a multi-agent or agent-benchmark claim with the question that dissolves most of them: *was this compared at equal token budget, and what did it cost per task?*
- **List** the production controls an agent needs before it touches anything real: bounded tools, tracing, a cost/latency budget, a test approach for a non-deterministic system, and a human gate.

## Prerequisites

- **Session 11 — Prompting II + Working With Claude.** This session assumes MCP: host, client, server, transports, tools-act-versus-resources-read, and the server as the enforcement point. **We do not re-teach MCP.** Session 11 taught you how to give a model a tool; this session is about what happens when you put that in a loop.
- **Session 10 — Prompting I: The Craft**, for prompt structure, output contracts, and the test-set discipline we reuse to test agents.
- **Session 2 — The Vocabulary and the Cost Meter**, specifically the agent cost multiplier (one user request → 8+ billed calls → roughly 13× cost). This session picks that number up and builds on it.
- **Session 1**, for the baseline that governs everything: the model is completing a pattern, and nothing in the mechanism checks that pattern against truth. A loop does not fix that. A loop compounds it.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–4 min | **Hook — three things called "an agent"** | Three real product descriptions on one slide. Two are workflows. Ask the room to sort them. Nobody agrees, which is the point. |
| 4–10 min | **What an agent actually is** | The loop diagram. Autonomy + decision-making + adaptation. The one-line definition: *an LLM wired to a loop and given the ability to act.* |
| 10–17 min | **Workflows vs. agents — the decision** | Predefined code paths vs. the model directing its own process. The decision flowchart. Why most things you want are workflows. |
| 17–25 min | **ReAct, built by hand** | Thought → Action → Observation drawn as a sequence diagram, then the ~40 lines of Python that implement it. The loop is visible; no framework. |
| 25–29 min | **Plan & Execute and Reflection** | Two variations, two architectures, and honestly when each stops paying. |
| 29–33 min | **When NOT to build an agent** | The most important slide. If a deterministic script works, use the script. Five task types, five verdicts. |
| 33–40 min | **The multi-agent evidence** | One lab's +90% claim, the same write-up's 80%-of-variance-is-tokens caveat at ~15× the tokens, another lab's "Don't Build Multi-Agents," that lab's later reversal, and the equal-token-budget research. Teach the disagreement. |
| 40–44 min | **Production: bound it, trace it, cost it, gate it** | The cost-per-pattern table. Non-determinism and testing. The failure modes. The human gate. |
| 44–45 min | **Hand-off to Session 14** | An agent is *an API acting on model output*. Say it, name it as the hazard, stop there. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. |

**Is 45 minutes honest?** It is tight but it holds, on one condition: **the ReAct segment must be a pre-baked walk-through, not a live run.** Capture a real trace in advance and reveal it step by step. A live agent run in front of a room will either take four minutes of silence or fail interestingly, and neither is the lesson. If you are behind at the 29-minute mark, cut Plan & Execute to a single sentence and protect the multi-agent segment — it is the part that changes how people read vendor material, and it is the part they cannot reconstruct from the reading. **Never cut "When NOT to build an agent."** For this audience it is the session.

## Materials & tools

- Slides: `slides/outline.md`, built per `../powerpoint_instructions.md`.
- Self-study reading: `content/00-overview.md` → `content/99-key-takeaways.md`. As always the reading is larger than the session and is the deliverable for anyone who missed the room.
- Lab: `exercises/lab.md` — build a ReAct loop from scratch in ~25 minutes, then break it three ways. Colab-first, JupyterLite fallback, plus a no-code trace-reading track for non-developers.
- Self-check: `exercises/quiz.md`. Discussion prompts: `exercises/discussion.md`.
- **Pre-baked trace (required).** Capture one complete agent trace — every thought, action, observation, and the token count per step — before the session. It is the centrepiece visual and it must not be generated live.
- **Sanitised data only.** Every tool, ticket ID, component name, and release number in these materials is invented.

## Source & licence note

| Source | Role in this session | Reuse verdict |
|---|---|---|
| **All code, diagrams, tables, and worked examples in this folder** | Written for this course | **Ours — SLIDE-SAFE without external attribution** |
| **Hugging Face AI Agents Course + `smolagents`** | The multi-framework, readable reference implementation participants are pointed at after the lab | **SLIDE-SAFE** — Apache-2.0, attribute |
| **Model Context Protocol specification** | The tool-transport layer this session assumes from Session 11 | **SLIDE-SAFE** — open standard, Agentic AI Foundation (Linux Foundation), attribute |
| **Anthropic Python SDK** | The Messages API patterns the lab code follows | **SLIDE-SAFE** — MIT (the library; the *documentation prose* is not) |
| **Anthropic — "Building Effective Agents"** and the multi-agent / context-engineering engineering posts | The workflow-vs-agent framing and the 90.2% / 80%-variance / ~15×-tokens data points | **LINK-ONLY** — proprietary. Paraphrase and attribute the concept; never reproduce text or figures |
| **Cognition — "Don't Build Multi-Agents"** and its 2026 follow-up | The opposing position, and the reversal | **LINK-ONLY** — paraphrase, attribute, do not quote onto a slide |
| **Neutral agent-scaling research** (equal-token-budget results; production-measurement studies) | The reality check that undercuts both vendor positions | **Cite the findings** — do not reproduce paper figures |
| **LangChain / LangGraph documentation and academy prose** | Mentioned as one framework among several, with the lock-in caveat stated | **LINK-ONLY** for prose (the libraries themselves are MIT) |

Full verdicts, URLs, and the "verify at delivery" register are in `resources/sources.md`.

> **⚠️ Currency warning.** Model names, framework APIs, and MCP details in this session drift fast. Everything product-specific is tagged **"verify at delivery."** In particular: **the MCP final specification publishes 2026-07-28** — if this session is delivered before that date, the MCP references inherited from Session 11 are against a release candidate, and you should say so.
