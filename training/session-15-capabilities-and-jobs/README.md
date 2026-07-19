# Session 15 — What AI Can and Can't Do — and Will It Take Our Jobs?

**Block:** Judge it · **Goals:** 8 (what AI can and cannot do) & 9 (will AI take our jobs) · **Format:** 45 min content + 15 min Q&A · **Hands-on:** none (this is a discussion session)

---

> ### ⚠️ Note to the training requester — candour level
> This session contains a **named, task-level analysis of four job families that people in the room actually hold**: release manager, problem manager, configuration manager, developer. It says out loud which sub-tasks are already automatable, which are not, and which get *harder*. It does not say "AI won't replace you, it will augment you" — that framing is a comfort blanket and an experienced room will see straight through it, which costs the whole series its credibility.
>
> **Before delivery, confirm the candour level with the training requester.** Three dials to agree explicitly:
> 1. **Headcount.** This draft says composition changes before headcount does, and names the conditions under which that stops being true (see `content/10-what-to-actually-do.md` §4). If the organisation has *already* made headcount decisions, delivering this text unchanged will read as either naïve or evasive. Ask.
> 2. **Naming internal tooling.** The role sections are deliberately generic (no internal tool names, no team names, no ticket volumes). Adding real examples makes the session dramatically better and dramatically more sensitive. Requester's call.
> 3. **Who is in the room.** This lands differently with a peer group than with a group that includes their line management. If managers are present, add one slide setting the ground rule that the discussion is about *task composition*, not about individuals.
>
> If the requester wants the session softened, soften the *framing*, not the *facts*. Removing the specifics leaves a vague reassurance session that is worse than not running it.

---

## Summary

This is the session the audience has been waiting for since Session 1, and it has two halves that only make sense together.

**Half A — capability and its ceiling.** We separate what large language models genuinely do well (transforming language from one form into another, drafting, summarising, spotting patterns across a lot of text) from what they structurally cannot do (novel reasoning, *guaranteed* correctness, anything that needs contact with real ground truth). Then the economic argument that governs everything downstream: capability does not grow exponentially — it follows an **S-curve**, and what grows exponentially is **the cost of closing the last gap**. That is why the demo is easy and production is hard, and it is why the **proof-of-concept-to-production gap** is real, durable, and — crucially — *precisely this team's professional turf*.

**Half B — will AI take our jobs?** Answered role by role, at task level, with no reassurance-noise. The useful frame is not "replaced vs. safe." It is three buckets: which sub-tasks get **automated**, which get **augmented**, and which get **harder** — because everyone else's output is now AI-shaped, and the volume of plausible-looking material you must verify has gone up. We work through release management, problem management, configuration management, and development concretely, and we finish with what to actually do: skills to build, what to delegate to AI, and what never to delegate.

The through-line: **AI changes the composition of these jobs long before it eliminates any of them, and the human moves up the stack toward judgement, verification, and accountability — which is where these four roles already live.** That is not a consolation prize. It is a description of who is well positioned and who is not.

## Audience & level

Qualcomm release, problem, and configuration managers plus developers. This session assumes the whole series behind it — especially Session 1 (an LLM is a pattern-completion engine, not a lookup engine), Session 13 (your metric is lying; base rates), and Session 14 (security, and the ~39% insecure-code-suggestion finding). No code is required. No maths beyond a percentage.

Emotionally, this is the hardest session in the series to deliver. It is also the one people will talk about afterwards. Deliver it flat and factual; the material carries its own weight, and any attempt to make it *feel* better makes it *land* worse.

## Learning objectives

By the end, a participant can:

- **Classify** a proposed AI use case into "genuinely well suited," "well suited with a verification gate," or "structurally unsuited," using the capability/ceiling table — and justify the classification.
- **Explain** the S-curve argument in their own words: that skill coverage saturates while cost per additional skill rises, and why this predicts an ~80%-coverage plateau rather than a march to 100%.
- **Describe** the proof-of-concept-to-production gap and name at least four things that live in it (data drift, outliers, monitoring, ownership, rollback).
- **Decompose** their own role into automate / augment / stays-human sub-tasks, and defend each placement.
- **Identify** which parts of their job get *harder* because of AI, not easier — and name the mechanism (verification load, AI-shaped input from everyone else, the verification paradox).
- **Decide**, for a concrete task, who owns the output — using the "task → who owns it" decision flow.
- **Name** three skills to build and three things never to delegate.

## Prerequisites

| Session | What this session assumes from it |
|---|---|
| **1** | "Autocomplete on steroids" — a pattern-matching engine, not a search engine looking up facts. Hallucination as pattern-completion outrunning evidence. |
| **2** | Tokens and cost. Half A's economic argument needs "cost scales with tokens." |
| **12** | Accuracy hides rare-event failure; base rates; the verification paradox (*if it's right 99% of the time, spotting the 1% is harder, not easier*). |
| **13** | Prompt injection has no clean fix; the ~39% insecure-suggestion finding; never let an automated pipeline act on model output without a qualified human gate. |

Sessions 10–11 (prompting, working with Claude) are helpful but not required.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook** | Two claims on one slide, both true, both from the last year. "AI writes 30% of our code." "Our AI pilot never shipped." Ask the room which one matches their experience. |
| 3–6 min | **The two questions** | Frame the session: *what can it do?* and *what does that mean for me?* State up front that the second half is specific and names roles. |
| 6–14 min | **What LLMs genuinely do well** | The four capability families, with the shared property: language-in, language-out, human verifies. The can/can't table. |
| 14–21 min | **Where they structurally fail** | Novel reasoning; guaranteed correctness; ground truth. Not "not yet" — *structurally*, from the mechanism in Session 1. |
| 21–28 min | **The S-curve and the last-mile cost** | The two curves. The ~80% plateau. "It is not capability that is exponential — it's the expense." Then the PoC-to-production gap: the demo is 20% of the work. |
| 28–30 min | **Turn** | So: if the last 20% is where the cost lives, and the last 20% is judgement, verification, and accountability — whose job is that? |
| 30–40 min | **The four roles** | Release / problem / configuration / developer, one slide each: automate · augment · gets harder · stays human. Fast, factual, no softening. |
| 40–45 min | **What to actually do** | Skills to build. What to delegate. What never to delegate. The one-line close. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. Budget the *full* 15 minutes; this session generates more questions than any other in the series. |

**Honest timing note.** This is the tightest 45 minutes in the series — it is two sessions' worth of material compressed because the halves are worthless apart. Two things protect the budget: (a) the four role slides must run at **2.5 minutes each, hard**, with the detail living in `content/06`–`content/09` for self-study; (b) resist taking questions during Half B — say so at the start ("hold them, we have fifteen minutes at the end and you will want them"). If you are running long at the 28-minute mark, cut the S-curve *diagram* discussion, not a role.

## Materials & tools

- Slides: `slides/outline.md`, built per `../powerpoint_instructions.md`.
- Self-study reading: `content/00-overview.md` → `content/99-key-takeaways.md`. **The role sections are the deliverable** — participants will re-read `content/06`–`content/09` after the session.
- `exercises/discussion.md` — the most important exercise file in the series. Live polls plus six non-defensive prompts.
- `exercises/lab.md` — no hands-on lab; contains a **structured self-audit** participants complete on their own role (~25 min). This is the take-home.
- `exercises/quiz.md` — 8 self-check questions with answers.
- No live demo, no notebook, no network dependency. This session runs in a room with a projector and nothing else.

## Source & licence note

Half A is grounded in Thomas Nield's *Deep Learning for Beginners* Day 3 (O'Reilly, **LINK-ONLY**) — the S-curve, selection bias, outliers, data rot, and the proof-of-concept-to-production gap — and in *LLM System Safety and Security* (O'Reilly, **LINK-ONLY**), which lists job losses from over-automation as a hazard outcome. Both are commercial, all-rights-reserved decks: **every idea here is re-authored in our own words and figures; nothing is reproduced.** The Andrew Ng radiology/data-drift remarks and the Chollet generalisation remarks are **paraphrased, never quoted on a slide** — attribute the idea, not the text.

Half B — the entire four-role analysis — is **original work authored for this course** and is therefore **SLIDE-SAFE**: we own it. The public labour-market framing (the automate/augment/harder decomposition) is likewise authored here.

The one external hard number is the ~39.33% insecure-suggestion finding (Pearce et al., *Asleep at the Keyboard?*, IEEE S&P 2022; arXiv preprint) — cite it with attribution, do not reproduce the paper's figures.

Full verdicts in `resources/sources.md`.
