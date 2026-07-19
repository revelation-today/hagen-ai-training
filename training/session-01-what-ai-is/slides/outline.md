# Slides — Session 1: What AI Is, and How It Relates to Human Thinking

Slide-by-slide spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, accessibility, licence-footer rules — not restated here). Target: **14 content slides** + title + agenda + Q&A + resources = **18 slides**, ~45 min. Speaker notes go in the Notes pane, never on the slide. Every headline is a claim, not a label.

Licence quick-reference for this deck:
- **SLIDE-SAFE (embed + attribute):** Maynez et al. 2020 (CC BY 4.0) — the intrinsic/extrinsic definitions and their diagram.
- **LINK-ONLY (never embed; paraphrase / live-demo / resources slide):** the AGI and LLM-safety source decks; cognitive-science papers; the FT explainer. All Mermaid below is **original to this course** and safe to render.

---

## Slide 1 — Title

- **On-slide text:** "What AI Is — and How It Relates to Human Thinking" · Session 1 of 15 · Block: *Understand it* · AI Training Series.
- **Speaker notes:** Welcome. This is the conceptual opener — no jargon, no code. Two ideas: AI learns like we do, and it fails like we do. By the end you'll have the one mental model the rest of the course is built on.
- **Visual:** Series title layout.
- **Source/licence:** none.

## Slide 2 — Agenda

- **On-slide text:** Learning by example vs. by rules · Memory is reconstructive · The flagship idea: hallucination = prejudice · Autocomplete on steroids · Q&A.
- **Speaker notes:** Walk the five beats. Flag that the middle beat — hallucination and prejudice as one failure mode — is the heart of the session. Mirror the minute-budget in `README.md`.
- **Visual:** Agenda layout matching the README table.
- **Source/licence:** none.

## Slide 3 — You have been confidently, vividly wrong (hook)

- **On-slide text:** "Have you ever remembered something *vividly* — and been *completely wrong*?" · Certainty is not a truth signal.
- **Speaker notes:** Show of hands. Take one 20-second story from the room. Land the point: you couldn't feel that the memory was false. Keep this — it's the emotional anchor for the whole session, and it makes the machine failure feel familiar rather than alien.
- **Visual:** Full-bleed question, minimal text.
- **Source/licence:** none.

## Slide 4 — AI turns programming inside out

- **On-slide text:** Classical: `data + rules → answers` · Machine learning: `data + answers → rules` · Nobody writes the rules — the machine infers them.
- **Speaker notes:** This one flip reframes the field. Classical software applies rules a human wrote. ML is handed the data *and the answers* and works out the rules itself. Those "rules" are a pile of numbers we call a model.
- **Visual:** Original Mermaid (the inversion), from `content/01`:
  ```mermaid
  flowchart LR
    subgraph CL["Classical software"]
      D1["Data"] --> P1["Rules a human wrote"] --> A1["Answer"]
    end
    subgraph ML["Machine learning"]
      D2["Data"] --> P2["Learning algorithm"]
      A2["Answers (labelled)"] --> P2 --> R2["Rules = a model"]
    end
  ```
- **Source/licence:** original diagram — safe. Alt text: two pipelines showing the direction of programming reversed.

## Slide 5 — Some rules can't be written — so we learn them

- **On-slide text:** Write the rule for "cat vs. dog in a photo." · You can't. A three-year-old can. · Fuzzy/perceptual → learn from examples. Few/stable → write the rules.
- **Speaker notes:** The cat/dog impossibility is the intuition pump. Then the discipline note this room will actually use: prefer the simplest thing that works — an auditable rule beats an opaque model when it can do the job. Callback promise: Sessions 5 and 12.
- **Visual:** Two-column: "Write the rules" vs. "Learn from examples" with the decision table from `content/01`.
- **Source/licence:** original.

## Slide 6 — The price of learning: the rule becomes unreadable

- **On-slide text:** The learned rule is *numbers*, not sentences · You can't glance at it and audit it · It only knows the answers you fed it.
- **Speaker notes:** Two costs to bank now: opacity (you can't read the rule) and dependence on the examples (skewed data → skewed rule). Every unsettling thing later grows from these two. Optional: mention the source decks' own light/dark label slip as proof even the authors couldn't glance at the rule and be sure which way it went — we corrected it; don't repeat it.
- **Visual:** The costs/benefits Mermaid from `content/01` (trimmed to ≤6 nodes).
- **Source/licence:** original.

## Slide 7 — Human memory is a rebuild, not a recording

- **On-slide text:** Memory ≠ video playback · Each recall *reconstructs* from fragments + expectations · Gaps get filled with the *plausible*.
- **Speaker notes:** Kill the camera metaphor. We store fragments and rebuild on demand, filling gaps with what usually happens — and we experience the rebuild as playback. Efficient, but lossy in a dangerous way.
- **Visual:** Original Mermaid (reconstruction pipeline) from `content/02`.
- **Source/licence:** concept after cognitive-science literature (LINK-ONLY) — diagram is original; state findings in our words, don't reproduce papers.

## Slide 8 — Confident false memories are easy to manufacture

- **On-slide text:** "Smashed" vs. "hit" changes remembered speed — and invents broken glass · Show a word list, people "remember" a word that was never there · Reported with *full confidence*.
- **Speaker notes:** Two findings (misinformation effect; DRM false-memory lists), stated plainly. The headline: you cannot tell a false memory from a true one by how it feels. Certainty is not a truth signal — same sentence as the hook.
- **Visual:** Two-panel: leading-question effect / word-list effect. Text only; no reproduced figures.
- **Source/licence:** Loftus; Roediger & McDermott — **LINK-ONLY** (findings paraphrased). If a % goes on the slide, verify against the paper at delivery.

## Slide 9 — An LLM generates a continuation; it does not retrieve a fact

- **On-slide text:** Human recall: fragments → plausible whole · LLM: prompt → probable next token · Neither has a built-in truth check.
- **Speaker notes:** Put the two side by side. Both fill a gap with the plausible; neither consults a verifiable record. When the pattern is strong, both are right; when it's thin, both are confidently wrong. This alignment is the ladder to the next slide.
- **Visual:** Original side-by-side Mermaid (human recall / LLM generation) from `content/02`.
- **Source/licence:** original diagram. The parallel is **after the AGI source deck (LINK-ONLY)** — attribute the framing verbally, don't show its slides.

## Slide 10 — Keep the caveat visible: it's an analogy, not an identity

- **On-slide text:** The memory ↔ hallucination link is an **analogy** · The mechanisms differ in detail · We name the limits of our own analogies.
- **Speaker notes:** Say it out loud: an LLM does not literally "reconstruct a memory" like a brain. Even the source author says so. We keep the caveat because (a) it's true and this room will push on it, and (b) naming our own limits is how the course earns trust. The analogy is a ladder to the real idea — next slide.
- **Visual:** A single honest caption slide; minimal.
- **Source/licence:** caveat after the AGI source (LINK-ONLY).

## Slide 11 — THE IDEA: hallucination = false memory = prejudice

- **On-slide text:** One failure mode: **pattern-completion outrunning the evidence** · Confidence tracks *plausibility*, not truth · Three coats, one bug.
- **Speaker notes:** The centre of the session — slow down. All three phenomena fill a gap with the most plausible pattern and report it with unwarranted confidence, because nothing checks the completion against evidence. This is the sharpest idea in the whole series; give it air.
- **Visual:** Original Mermaid (the mechanism with the missing evidence-check `Q`) from `content/03`:
  ```mermaid
  flowchart TD
    P["A gap / unknown"] --> M["Complete the pattern:<br/>most plausible fill"]
    M --> Q{"Evidence supports it?"}
    Q -->|Yes| G["Useful inference"]
    Q -->|"No — but no check"| B["Confident error"]
    B --> H["Hallucination"]
    B --> F["False memory"]
    B --> J["Prejudice"]
  ```
- **Source/licence:** original synthesis authored for this course. Alt text: one mechanism branching into three named failures.

## Slide 12 — Prejudice is that mechanism, pointed at people

- **On-slide text:** Prejudice = over-generalisation from **skewed data** · A biased hiring model *hallucinates* a verdict about a person · "It's just pattern recognition" — yes, and that's the problem.
- **Speaker notes:** Mechanically identical to hallucination, aimed at an individual instead of a fact. The model inherits the skew in its examples and launders it as objectivity (callback to cost C2, slide 6). Bias is not exotic; it's hallucination with a demographic target — so the same habit (verify, don't trust) counters both.
- **Visual:** Original Mermaid (skewed data → learned rule → confident unfair judgement) from `content/03`.
- **Source/licence:** original.

## Slide 13 — Name the wrongness precisely: intrinsic vs. extrinsic

- **On-slide text:** **Intrinsic** = output *contradicts* the source · **Extrinsic** = output *can't be verified* from the source · Grounding/RAG mainly targets the extrinsic kind.
- **Speaker notes:** This is the one slide with a citable, slide-safe definition — use it. Maynez et al. 2020 split hallucination in two. It matters because it tells you *where* to put the check: grounding gives the model a source so answers can be verified against it, but a grounded model can still contradict its source. Sets up Session 13.
- **Visual:** Original Mermaid taxonomy from `content/04`:
  ```mermaid
  graph TD
    H["Hallucination"] --> I["Intrinsic: contradicts the source"]
    H --> E["Extrinsic: can't be verified from the source"]
  ```
- **Source/licence:** **Maynez et al. 2020, ACL Anthology — CC BY 4.0. SLIDE-SAFE.** Footer attribution required: "Definitions: Maynez et al. 2020 (CC BY 4.0)."

## Slide 14 — The dangerous failures are the *plausible* ones

- **On-slide text:** Obvious errors are safe — you discard them · Plausible falsehoods slip through · Human-in-the-loop is necessary, **not sufficient**.
- **Speaker notes:** Confidence + no evidence-check = failure in the most expensive way: plausibly. A fabricated-but-formatted citation passes review precisely because it looks like the real ones. This is why later sessions don't stop at "put a human in the loop." Foreshadow Session 13's "if it's right 99% of the time, spotting the 1% is harder, not easier."
- **Visual:** Simple contrast: "obvious error → discarded (safe)" vs. "plausible error → accepted (dangerous)."
- **Source/licence:** framing after the LLM-safety source (LINK-ONLY) — paraphrased.

## Slide 15 — The model: autocomplete on steroids

- **On-slide text:** It **generates** the next likely token · It does **not** look facts up · A pattern-matcher, not a search engine.
- **Speaker notes:** Search retrieves stored documents; an LLM generates a probable continuation, one token at a time, feeding itself back in. Scaling autocomplete up made it astonishing — but it added no fact-checker. A superb next-token predictor is still a next-token predictor. Optional live demo cue (presenter's own sandbox only): ask a model to describe a made-up person; watch it invent a confident CV. Do **not** use a real colleague's name.
- **Visual:** Original Mermaid (generation loop) from `content/04`. Live-demo cue is **not** an embedded asset.
- **Source/licence:** "autocomplete on steroids / pattern-matcher not search engine" — framing after the LLM-safety source (LINK-ONLY); phrase is common parlance, delivered in our own words.

## Slide 16 — Everything else in the course follows from this

- **On-slide text:** Predicts tokens → **costs** by the token (S2) · No truth-check → **risk**, verify don't trust (S12–13) · Language engine, not truth engine → **capability & jobs** (S14).
- **Speaker notes:** Close the loop. This one sentence is load-bearing for the whole series: cost, risk, and capability are all consequences of "pattern-matcher, not search engine." Session 9 will show you the machinery (attention); today you have the intuition.
- **Visual:** Original Mermaid (the bridge fan-out) from `content/04`.
- **Source/licence:** original.

## Slide 17 — Q&A / discussion

- **On-slide text:** "Where have you seen a system be *confidently* wrong? Was it *lying*, or *reconstructing*?" · Poll: lying / reconstructing / other.
- **Speaker notes:** Run the seed question from `exercises/discussion.md`. Steer toward "reconstructing" — it can't lie, it has no known truth to hide. Use the room's own examples (a wrong dashboard, a bad autocomplete, a biased screen) to rehearse the habit-of-mind question.
- **Visual:** Discussion/poll layout.
- **Source/licence:** none.

## Slide 18 — Resources & credits

- **On-slide text:** Maynez et al. 2020 (CC BY 4.0) — intrinsic/extrinsic · Further reading: cognitive-science false-memory papers; source decks (internal). · Full list: `resources/sources.md`.
- **Speaker notes:** Point to the reading. Only Maynez is reproduced on slides; everything else is link/assign-only. Attribution and licences live in `resources/sources.md`.
- **Visual:** Resources & credits layout with licence attributions.
- **Source/licence:** as listed; SLIDE-SAFE items attributed, LINK-ONLY items linked only.

---

### Build checklist (this deck)
- [ ] 14 content slides (4–16); none over 6 bullets; every headline a claim.
- [ ] Only Slide 13 embeds a sourced definition (Maynez, CC BY 4.0) — footer attribution present.
- [ ] No source-deck slide art reproduced; all diagrams are the original Mermaid above.
- [ ] Alt text on every diagram; greyscale-safe; 18 pt minimum.
- [ ] The light/dark label error is corrected, not repeated, if slide 6 mentions it.
- [ ] Runs in ~45 min rehearsed; Slide 3 hook and Slide 11 idea get the most air.
