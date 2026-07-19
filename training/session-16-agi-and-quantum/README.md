# Session 16 — What Is AGI, and an Outlook on Quantum

**Series:** AI Training for Qualcomm (Release / Problem / Configuration Management + Developers)
**Block:** Judge It (Sessions 15–15) · **Goals 10 & 11** (what is AGI · outlook: impact of quantum computers)
**Format:** 45 min content + 15 min Q&A · English · Python optional (one tiny illustration)
**Position:** **the series closer.** Last session of 15.

---

## One-paragraph summary

This is the session where we look at the horizon — and refuse to squint. The first ~30 minutes are about **AGI**: a term that has been redefined roughly every decade since 1950, and whose current definitions are written largely by the organisations selling the thing being defined. We trace that moving target (Turing → Newell & Simon → Legg & Hutter → Goertzel → Chollet → OpenAI → ARC-AGI), then use a **seven-pillar model of human intelligence** — reasoning, memory, learning, language, perception, self-awareness, motivation/values — as a checklist for what today's systems demonstrably have and demonstrably lack. Crucially, we do this with **evidence, not vibes**: reasoning models that fail puzzles a child solves, a "world model" probe that beats random guessing by ten percentage points, reasoning models that hallucinate *more* than the non-reasoning model they replaced, and a benchmark (ARC-AGI-2) humans pass and machines do not. We finish the AGI half with the strongest available argument against hype: **the frontier labs themselves do not agree that AGI is coming**, and five structural limits of the Transformer architecture that no amount of scaling obviously removes. The last ~15 minutes are the **quantum outlook** — what a qubit actually is, why a quantum computer is not "a faster computer", where it might one day touch AI, and the honest bottom line for this room: *near-term impact on your work is minimal, the AI intersection is speculative, and the one thing that is genuinely on your roadmap is post-quantum cryptography.* Then we close the series.

## Audience & level

Qualcomm release / problem / configuration managers and developers. By Session 16 the room has the full vocabulary of the series: tokens, context windows, training vs. inference, hallucination, benchmarks, agents, the S-curve, the proof-of-concept-to-production gap. **No new mathematics is introduced.** The session is conceptual and evidence-led; the demands it makes are on judgement, not on technique.

**Role hook:** this audience's professional instinct — *"show me it working in production, not in the demo"* — is exactly the right instinct for AGI claims and for quantum claims. This session gives them the specific questions to ask when a vendor, a headline, or an internal enthusiast makes a horizon claim.

## Learning objectives

By the end of this session a participant can:

1. **Trace** how the definition of AGI has shifted from 1950 to 2025, and **explain** why the current definitions favour their authors.
2. **Use** the seven-pillar framework to say concretely which components of intelligence current systems have, partially have, and lack.
3. **Cite** at least three specific pieces of counter-evidence to "AGI is imminent" — with the caveat that each number must be re-verified at delivery date.
4. **Summarise** the disagreement between frontier labs and explain why unanimity would be more suspicious than disagreement.
5. **Name** the five structural limits of the Transformer architecture and say why each is not obviously fixed by scaling.
6. **Explain** at a high level what a qubit, superposition and entanglement are, and **state** honestly why quantum computing is not near-term relevant to their daily work — with the exception of post-quantum cryptography.
7. **Apply** a four-question filter to any horizon claim (AGI, quantum, or the next thing).

## Prerequisites

- **Session 1** — what AI is; hallucination as reconstructive pattern-completion. This session closes the loop opened there.
- **Session 9** — how LLMs work (attention, tokens, context window). Required for the "limits of the Transformer" segment.
- **Session 13 / 14** — benchmark skepticism, the S-curve, the capability ceiling. Session 16 extends both to the horizon.
- No new maths. No prior physics. The quantum segment assumes nothing beyond "a bit is a 0 or a 1".

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook** — the word that keeps moving | Ask the room to define AGI. Collect three incompatible answers. That *is* the lesson. |
| 3–10 min | **The moving definition** | The 1950→2025 timeline table; who wrote each definition and what it served. |
| 10–17 min | **Seven pillars of intelligence** | The framework; the concept map; what each pillar means for a machine. |
| 17–27 min | **The evidence — what is still missing** | The gap table + the four hard results (Hanoi, chess probe, hallucination regression, ARC-AGI-2). |
| 27–32 min | **The labs disagree** | Four positions, four quotes (paraphrased on slides), and why disagreement is informative. |
| 32–36 min | **Five limits of the Transformer** | O(n²), shallow reasoning, rigidity, no embodiment, monoculture. |
| 36–44 min | **Quantum outlook** ⚠️ speculative | Qubits/superposition/entanglement; not-a-faster-computer; NISQ + error correction; the AI intersection; post-quantum crypto. |
| 44–45 min | **Series close** | Callback to Session 1; where to go next. |
| 45–60 min | **Q&A** | See `exercises/discussion.md`. |

**Honest timing note.** This session is the tightest in the series — it carries two goals and closes fifteen weeks of material. The AGI half alone could fill 45 minutes. Two planned cuts, in order: (1) drop the classical/modern philosophy beats of the seven pillars and present the pillars as a bare checklist (saves ~4 min, costs depth); (2) compress the five Transformer limits to three (O(n²), no embodiment, monoculture — saves ~2 min). **Do not** cut the evidence segment (17–27) — it is the reason the session exists. If quantum is running over, drop the "where it might intersect AI" detail and keep the timeline plus post-quantum crypto; those are the two parts that matter to this room.

## Materials & tools

- **Self-study reading:** `content/00-overview.md` → `content/99-key-takeaways.md`, in order. The reading is fuller than the delivered session by design — it is the durable artefact.
- **Optional 10-minute code illustration:** `exercises/lab.md` — a tiny ARC-style grid puzzle in pure Python, to make "generalisation beyond training data" tangible. Not a full lab; this is a concept session.
- **Deck spec:** `slides/outline.md` (built per `../../powerpoint_instructions.md`).
- **Live demo (optional, 2 min):** the ARC-AGI public task viewer / repository — open-licensed, safe to show and screenshot.
- **Discussion:** `exercises/discussion.md` — includes the closing series-wide prompt.

## ⚠️ Two delivery notes for the requester

**1. Everything numeric in this session must be re-verified on the delivery date.** Benchmark scores, model names, lab leadership and lab positions move faster here than anywhere else in the series. Every such claim in `content/` and `slides/outline.md` is tagged **`[verify at delivery]`**. A stale ARC-AGI score in a session *about* benchmark skepticism is an own goal. Budget 30 minutes of re-checking before you present.

**2. The quantum segment's emphasis should be tuned to Qualcomm's remit — please steer.**
This folder authors the quantum segment as a **balanced general outlook**, weighted slightly toward the two angles most defensible for this audience: **hardware/engineering realism** and **post-quantum cryptography**. Depending on what is actually relevant inside Qualcomm, the emphasis should shift:

| If the relevant angle is… | Then expand… | And compress… |
|---|---|---|
| **Post-quantum cryptography** (most likely — it affects shipped products, firmware signing, key exchange, long-lived device fleets) | `content/06` §6 — migration timelines, "harvest now, decrypt later", the standardised algorithm families, what a config/release manager owns in a crypto migration | the quantum-ML section to two sentences |
| **Quantum hardware / device physics** (if there is an internal research interest) | qubit modalities, coherence, error-correction overhead, the engineering reality of cryogenics and control electronics | the AI-intersection section |
| **Quantum ML** (least defensible — say so) | nothing; keep it short and label it research-stage | — |
| **General horizon scan** (the default authored here) | leave as written | — |

The proposal flagged this as **open decision #2** (`../../training_proposal.md` §7). It remains open. **The requester should steer before this deck is built.**

## Source & licence note

The AGI half draws on the corpus's largest and most current source — the **AGI Demystified** deck (Sinan Ozdemir, O'Reilly, ~2026, 212 pages; see `../../AI_input.md` §2.5). That deck is **LINK-ONLY**: all-rights-reserved commercial training material. Its *structure* (the seven-pillar framing, the definition timeline, the honest-negative-results posture) is used as an organising idea and rewritten entirely in our own words; **no slide, quote, figure or phrasing is reproduced.** Quoted lab positions are **paraphrased** on slides and attributed to the speaker, not to the deck. Primary sources are linked directly wherever they exist.

**SLIDE-SAFE:** ARC-AGI (Chollet, `github.com/fchollet/ARC-AGI`, Apache-2.0 — tasks and figures may be shown); our own diagrams, tables and code; NIST post-quantum standards (US government, public domain).
**LINK-ONLY:** the AGI source deck; lab blog posts and papers (Anthropic, Meta, Mistral, OpenAI, DeepMind, Apple); all quoted material — paraphrase unless explicitly CC-licensed.

Full verdicts in `resources/sources.md`.
