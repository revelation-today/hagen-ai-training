# AI Training Series

Internal AI training course for a Qualcomm team (release management, problem management, configuration management, and developers).

**16 sessions · 45 minutes content + 15 minutes Q&A each · English · Python throughout.**

---

## What's here

| Path | What it is |
|---|---|
| [`training/`](training/) | **The course.** 16 session folders, each with self-study reading, a slide spec, exercises, and sources. |
| [`training/README.md`](training/README.md) | Course index and session list — **start here**. |
| [`training/powerpoint_instructions.md`](training/powerpoint_instructions.md) | How to build the 16 PowerPoint decks from the session outlines. |
| [`training/_TEMPLATE/SESSION_STRUCTURE.md`](training/_TEMPLATE/SESSION_STRUCTURE.md) | Authoring spec every session follows. |
| [`training_proposal.md`](training_proposal.md) | Why the course is shaped this way: goal mapping, source analysis, effort estimates. |
| [`AI_input.md`](AI_input.md) | Consolidated extract of the source material the course was built from, incl. error and currency registers. |

## Session list

| # | Session | Block |
|---|---|---|
| 1 | What AI Is, and How It Relates to Human Thinking | Understand |
| 2 | The Vocabulary, and the Cost Meter | Understand |
| 3 | Methods I — Learning From Data | Methods |
| 4 | Methods II — Unsupervised Learning | Methods |
| 5 | Methods III — Decision Trees & Random Forests | Methods |
| 6 | Methods IV — Deep Learning, Conceptually | Methods |
| 7 | Hands-On I — Build & Train a Network in Keras | Do |
| 8 | Hands-On II — Make It Better | Do |
| 9 | How LLMs Work — From Neural Networks to Claude | Do |
| 10 | Prompting I — The Craft | Use well |
| 11 | Prompting II + Working With Claude | Use well |
| 12 | Agents and Tool Use | Use well |
| 13 | Risk I — When AI Is Confidently Wrong | Use safely |
| 14 | Risk II — Security, Privacy & Mitigation | Use safely |
| 15 | What AI Can and Can't Do — and Your Jobs | Judge |
| 16 | What Is AGI, and an Outlook on Quantum | Judge |

Run in numeric order — the series is built as a journey: understand it → know the methods → do it → use it well → use it safely → judge it.

**Recommended pilot: Session 13** — no lab, no currency-sensitive content, and it carries the strongest single teaching asset in the course (taking apart a vendor's "99% accurate" claim).

## Conventions

- **Code:** Python only (numpy, scikit-learn, tensorflow.keras, Anthropic/OpenAI SDKs), with expected output in comments.
- **Diagrams:** Mermaid, rendered inline (~296 diagrams). Tables used liberally.
- **Voice:** technical, honest, vendor-neutral, skeptical of hype — distinguishes demo from production.

## Licence discipline — read before building slides

This material is for internal use. Slide and content text/figures are derived **only** from reuse-safe sources: permissive code licences, CC-BY/BSD, government and standards bodies, and explicitly CC-licensed course material. High-quality but all-rights-reserved material (3Blue1Brown, Jay Alammar's *Illustrated Transformer*, StatQuest, personal blogs) is **linked as reading or demoed live, never copied onto a slide**.

Every session's `resources/sources.md` records a per-source verdict: `SLIDE-SAFE` or `LINK-ONLY`. When in doubt, treat as LINK-ONLY.

One source deck available during authoring carried another company's confidentiality marking and was **excluded entirely**. The topics it covered (unsupervised learning, trees/forests, transformer internals) were rebuilt from clean public sources — see `training_proposal.md`.

## Open items before delivery

- **Session 15** (jobs): confirm the candour level with the training requester — its `README.md` lists three specific dials.
- **Labs** assume Google Colab; JupyterLite is the no-account fallback if corporate policy blocks external notebook hosting.
- **Session 16** quantum segment: tune emphasis to the team's remit; it is flagged as the most speculative content in the series.
- **Currency:** pricing, model names, framework APIs, OWASP versions and EU AI Act dates all drift. Anything time-sensitive is marked *"verify at delivery"*.
