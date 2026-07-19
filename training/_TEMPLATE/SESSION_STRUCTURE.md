# Session Authoring Spec — READ BEFORE WRITING ANY SESSION

This file defines the exact folder layout, file set, house style, and quality bar for every training session in `output/training/`. Every session folder must follow it identically so the 15 sessions read as one course.

---

## 1. Folder layout (per session)

```
session-NN-<slug>/
  README.md                  # session cover sheet — the single entry point
  content/                   # the self-study material (the substance)
    00-overview.md
    01-<topic-slug>.md
    02-<topic-slug>.md
    ...                      # one file per major topic; 3–7 files typical
    99-key-takeaways.md
  slides/
    outline.md               # slide-by-slide spec for THIS session's PowerPoint
  exercises/
    discussion.md            # Q&A-block prompts and live-poll questions
    lab.md                   # hands-on activity (Python); omit body with a note if the session has no lab
    quiz.md                  # 5–10 self-check questions WITH answers
  resources/
    sources.md               # every source used, with URL + licence status + reuse verdict
```

Do not invent extra top-level files. Extra `content/` topic files are fine and expected.

---

## 2. What each file must contain

### `README.md` (cover sheet)
- **Session number and title**
- **One-paragraph summary** — what this session is and why it matters to the audience.
- **Audience & level** — Qualcomm release / problem / configuration managers + developers; some prior AI exposure. Note anything role-specific.
- **Learning objectives** — 4–6 bullets, each starting with a verb ("Explain…", "Build…", "Decide…"). Objectives must be checkable.
- **Prerequisites** — which earlier sessions or skills are assumed.
- **Agenda (45 min + 15 min Q&A)** — a minute-budgeted table (e.g. `0–5 min | Hook | …`). The content must fit 45 minutes of delivery; say so honestly if it's tight.
- **Materials & tools** — notebooks, live demos, links used.
- **Source & licence note** — which source decks/public material this session draws on, and the reuse verdict (build-slides-from vs link-only). Pull from `resources/sources.md`.

### `content/*.md` (the self-study reading — THIS IS THE MAIN DELIVERABLE)
- Written for **self-study**: a reader who missed the live session should learn the topic fully from these files alone.
- **High detail.** Explain mechanisms, don't just name them. Worked examples with numbers. Analogies where they earn their place.
- Every file starts with an `# H1` title and a 1–2 sentence orientation.
- Use tables, and fenced code blocks. **All prose and all examples in English.**
- **All code examples in Python.** Prefer runnable snippets (numpy / scikit-learn / tensorflow.keras / the Anthropic or OpenAI SDK as relevant). Show expected output in a comment.
- Where a claim comes from a specific source, cite it inline as `(see resources/sources.md #N)`.
- Call out the source-deck errors where relevant (see `output/AI_input.md` §6) rather than repeating them.
- `00-overview.md` sets up the session's arc; `99-key-takeaways.md` is a tight bulleted recap + "if you remember one thing" line.

### `slides/outline.md` (the PowerPoint spec for this session)
- A **slide-by-slide** outline the deck-builder follows. Target **12–18 content slides** for 45 minutes (≈2–3 min/slide) plus title, agenda, Q&A, and resources slides.
- For **each slide** give a table row or block with:
  - `Slide N — <title>`
  - **On-slide text**: the actual headline + ≤6 concise bullets (what the audience sees).
  - **Speaker notes**: 3–6 sentences the presenter says (this is where detail lives; slides stay sparse).
  - **Visual**: the diagram/screenshot/demo to show, and where it comes from.
  - **Source/licence**: if the visual/text derives from a source, name it and its reuse status. If a visual is link-only (e.g. 3Blue1Brown), mark it **"live-demo/link only — do not embed"**.
- Follow the design system in `../powerpoint_instructions.md` (do not restate it — reference it).

### `exercises/`
- `discussion.md` — the questions for the 15-minute Q&A and any in-session polls. Give 4–8 prompts, each with a sentence on what a good answer surfaces.
- `lab.md` — for hands-on sessions (7, 8, and any with code): a step-by-step Python lab a participant can complete in ~20–30 min, with setup (Colab-first; note the JupyterLite fallback), the code, expected output, and 2–3 "now break it / now extend it" challenges. For sessions with no lab, keep the file but state "No hands-on lab — this is a concept session" and give one reflective exercise instead.
- `quiz.md` — 5–10 self-check questions with an answer key at the bottom.

### `resources/sources.md`
- Numbered list. For each: title, author/org, URL, date/version, licence, and a one-line **reuse verdict**: `SLIDE-SAFE` (permissive/CC-BY/BSD/public-domain — may derive slides with attribution) or `LINK-ONLY` (all-rights-reserved / NC / ND — assign as reading or live-demo, never copy onto a slide).
- Include the relevant source-deck provenance and any research findings used.
- End with a **"Further reading"** subsection of the LINK-ONLY high-quality material.

---

## 2b. Visual aids (required)
Markdown files must be **visually rich** for readability, not walls of prose:
- **Tables** — use liberally for comparisons, taxonomies, parameter lists, decision matrices, before/after, cost breakdowns. Most `content/*.md` files should contain at least one table.
- **Diagrams and flowcharts — use Mermaid** (```mermaid fenced blocks; they render on GitHub, in VS Code with the Mermaid extension, and in Artifacts). Every content file that describes a process, architecture, pipeline, decision, or relationship must include at least one Mermaid diagram. Use the right type:
  - `flowchart TD/LR` — pipelines, decision trees, "how a request flows", when-to-use decision logic
  - `sequenceDiagram` — multi-step interactions (agent loops, RAG retrieval, tool calls)
  - `graph` / concept maps — taxonomies and relationships (AI⊃ML⊃DL⊃LLM, the hazard triangle)
  - `xychart-beta` or a table — for curves like the S-curve, loss over epochs, cost vs. context length
- Keep diagrams legible: ≤ ~12 nodes each; split a big one into two rather than cramming.
- Label every diagram with a one-line caption above it.
- In `slides/outline.md`, when a diagram belongs on a slide, provide the Mermaid source in the slide's **Visual** field so the deck-builder can render or redraw it.

## 3. House style
- Audience is technical but not all coders. Explain jargon on first use. Respect their intelligence; skip hype.
- The course has a **skeptical, honest voice**: name what AI can't do, flag uncertainty, distinguish demo from production. Preserve it.
- Prefer concrete numbers and worked examples over adjectives.
- Neutral on vendors. When a source is vendor material, say so.
- Gender-neutral; use they/them for unspecified people.
- Every code block: Python, English comments, expected output shown.
- Never present a source-deck error as fact; correct it and note the correction.

## 4. Licence discipline (non-negotiable)
This deck is for internal Qualcomm training. Only derive slide/content text and figures from **SLIDE-SAFE** sources (permissive code licences, CC-BY/BSD, government/standards bodies, explicitly CC-licensed course material). Everything else is **LINK-ONLY**: reference it, assign it as pre-reading, or show it as a live demo — never reproduce it. When in doubt, treat as LINK-ONLY. The `resources/sources.md` reuse verdict governs.
