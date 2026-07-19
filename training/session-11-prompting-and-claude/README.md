# Session 11 — Prompting II + Working With Claude

**Block:** Application · **Goals:** 5 (prompting craft) & 6 (get value from Claude) · **Format:** 45 min content + 15 min Q&A · **Hands-on:** yes — a short lab (`exercises/lab.md`), plus two live demos

---

## Summary

Session 10 taught the moves: few-shot, chain-of-thought, delimiters, system prompts, structured output. This session is about **making them stick on your actual work.** The first half takes four tasks this team really does — drafting release notes, summarising an incident, reviewing a configuration change, and triaging a log — and walks each one from a lazy prompt to a prompt you would be willing to check into a repository. Then it does the thing that turns prompting from folklore into engineering: we build a small **prompt test set**, ten to twenty cases drawn from real failures, and we run a prompt against it. Once a prompt has a test set, it has a version number, a pass rate, and a reason to change — and "the new prompt feels better" stops being an acceptable argument.

The second half is about **Claude as a working environment rather than a chat box.** The people who get value out of Claude and the people who do not are usually running the same model; the difference is workflow. So we cover the durable habits — put the stable context in first and the variable part last, keep a scratchpad, make the model produce an artifact you can diff, ask for the reasoning budget you actually need — and we map Claude's surfaces (chat, Projects, Artifacts, extended thinking, the API, connectors/MCP) onto which of your tasks each one actually suits.

**A standing warning that governs the whole second half:** Claude's product surface moves faster than any training deck. Every product-specific claim in these materials is tagged **"verify against current Claude documentation at delivery"**. Treat the workflow principles as the content and the feature names as perishable packaging.

## Audience & level

Qualcomm release / problem / configuration managers and developers. Everyone will have used a chat assistant; not everyone codes. The Python examples (Anthropic SDK) are for the developers and are fully explained in the reading, but the session's core — prompt structure, test sets, workflow habits — requires no code at all. The non-coders should follow the lab's no-code track.

## Learning objectives

By the end, a participant can:

- **Rewrite** a vague request into a structured prompt with role, context, task, constraints, and an explicit output contract — demonstrated on a release-notes or incident-summary task.
- **Diagnose** a failing prompt by category (missing context, ambiguous task, unconstrained output, wrong model, wrong tool) rather than by adding words at random.
- **Build** a prompt test set of 10–20 cases from real failures, define pass/fail criteria for each, and report a prompt's pass rate.
- **Decide** which Claude surface — chat, Project, Artifact, extended thinking, API, connector — fits a given task, and justify the choice.
- **Explain** what MCP is at the protocol level (client, server, transport, stateless core) and state when a connector is and is not worth wiring up.
- **Apply** at least three workflow habits (stable-prefix ordering, scratchpad, artifact-and-diff) to a task from their own week.

## Prerequisites

- **Session 10 — Prompting I: The Craft.** This session assumes the vocabulary (zero-/few-shot, chain-of-thought, system prompt, delimiters, structured output) and the define → draft → test → refine loop. We do not re-teach them; we apply them at length.
- **Session 9 — How LLMs Work** for context windows, tokens, and why ordering inside the prompt has cost consequences.
- **Session 1** for the honest baseline: the model is completing a pattern and nothing in the mechanism checks that pattern against truth. Every workflow habit in the second half exists because of that fact.
- Useful but not required: Python literacy, and an Anthropic API key for the optional coding track of the lab.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook** | Two prompts for the same release-notes task, side by side, with their real outputs. Same model. The gap is entirely the prompt. |
| 3–12 min | **Worked example 1 — release notes** | Build the prompt live, layer by layer: role → context → task → constraints → output contract. Show what each layer fixes. |
| 12–19 min | **Worked example 2 — incident summary and log triage** | The two hardest cases: one where the input is long and messy, one where the model must say "insufficient evidence" instead of guessing. |
| 19–27 min | **Iterating a bad prompt, live** | A deliberately bad config-review prompt. Diagnose by category, fix one thing per pass, three passes. Audience calls the diagnosis. |
| 27–35 min | **Prompt test sets** | Ten cases from real failures. Pass/fail criteria. Run it. Watch a "better" prompt regress a case nobody would have caught by eye. |
| 35–42 min | **Working with Claude** | The surface decision table; scratchpad; Projects; Artifacts; extended thinking. Demo-led. |
| 42–45 min | **MCP in one honest slide + habits recap** | What a connector actually is, when it earns its keep, and the four habits worth stealing. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. |

**Honest timing note: this is the tightest session in the series.** It carries two halves that each deserve 45 minutes. The minute budget above works only if the worked examples are pre-baked (prompts and outputs captured in advance, revealed rather than typed) and the live iteration is rehearsed to three passes and no more. If you are running behind at the 27-minute mark, cut the second worked example to a single slide and protect the test-set segment — it is the part participants cannot reconstruct from the reading on their own. If your audience is heavily non-coding, consider splitting this into two 45-minute sittings rather than compressing.

## Materials & tools

- Slides: `slides/outline.md`, built per `../powerpoint_instructions.md`.
- Self-study reading: `content/00-overview.md` → `content/99-key-takeaways.md`. The reading is deliberately larger than the session; it is the deliverable for anyone who missed the room.
- Lab: `exercises/lab.md` — build and run a prompt test set. Colab-first, with a no-code spreadsheet track for non-developers and a JupyterLite fallback.
- Self-check: `exercises/quiz.md`. Discussion prompts: `exercises/discussion.md`.
- **Live demos (never screenshotted into the deck):** a Claude Project set up with a fake but realistic release-context document; an Artifact generated and then revised; extended thinking on a config-conflict question. All are product UI — treat as live-demo-or-link only.
- **Sanitised data only.** Every example in this session uses invented components, invented CVE-style identifiers, and invented ticket numbers. Nothing real goes into a demo. See the note in `content/08-workflow-habits.md` on what must never be pasted.

## Scheduling constraint (read before booking the room)

The **MCP final specification publishes 2026-07-28.** If this session covers connectors — and the outline does — **land it after that date.** Delivering it earlier means teaching against a release candidate and telling the room that the thing they just learned changes in a fortnight, which is the fastest way to lose an engineering audience. If the session must run earlier for scheduling reasons, deliver the MCP segment as the stateless conceptual core only (client / server / transport / tools-and-resources) and explicitly defer the specifics.

## Source & licence note

The prompting half is **original work**: every prompt, every worked example, every test case in these files was written for this course and is therefore **SLIDE-SAFE** without qualification. Where it rests on published technique taxonomies, it draws on the **SLIDE-SAFE** sources established in Session 10 — the OpenAI Cookbook prompting guides (MIT), DAIR.AI's promptingguide.ai (MIT), and *The Prompt Report* v6 (CC BY 4.0).

The Claude half is **fully authored** — there is no corpus source for it. Anthropic's product documentation and engineering blog are **LINK-ONLY** (proprietary, no open licence): we link them, we paraphrase concepts in our own words, we never lift text or figures. **MCP is the exception** — it is an open standard under the Agentic AI Foundation (Linux Foundation), and the specification and its official SDKs are **SLIDE-SAFE**; the protocol diagram in these materials is drawn from the spec's own architecture, in our own rendering.

Full verdicts, URLs, and the "verify at delivery" register are in `resources/sources.md`.
