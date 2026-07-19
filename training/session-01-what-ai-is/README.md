# Session 1 — What AI Is, and How It Relates to Human Thinking

**Block:** Understand it · **Goal covered:** 1 · **Format:** 45 min content + 15 min Q&A

---

## One-paragraph summary

This is the opener, and it deliberately uses no jargon. It builds one core analogy and one core disanalogy between machine learning and human thinking. The analogy: modern AI *learns by example*, the way people do — you invert classical programming from "data + rules → answers" to "data + answers → rules." The disanalogy is the one that matters most for the rest of the series: an LLM does not *retrieve* facts, it *generates* a plausible continuation — and that is exactly what human memory does when it reconstructs the past and produces a confident false memory. Framing **hallucination and human prejudice as the same failure mode** — pattern-completion running ahead of the evidence — is the sharpest idea in the whole brief. The session lands on the single most useful mental model in the series: *an LLM is autocomplete on steroids — a pattern-matcher, not a search engine.*

## Audience & level

Qualcomm release / problem / configuration managers and developers, with some prior AI exposure. This session assumes **no** machine-learning background and introduces no code you must run. It is a concept session; the payoff is a set of mental models the later, more technical sessions build on. Managers and developers get equal value — the failure modes named here (confident wrongness, skewed-data bias, verification burden) are exactly what a release/problem/config discipline exists to catch.

## Learning objectives

After this session a participant can:

1. **Explain** the inversion at the heart of machine learning — traditional software is *data + rules → answers*; ML is *data + answers → rules* — and give one example of a problem where learning-by-example beats writing rules.
2. **Describe** why human memory is reconstructive rather than a recording, and why that produces *confident* errors.
3. **Explain** how an LLM produces text (a generated continuation, not a retrieved fact) and why that is structurally the same move as a reconstructed memory.
4. **Argue** that hallucination, confident false memory, and prejudice are one failure mode — pattern-completion outrunning evidence — and identify that mechanism in a fresh example.
5. **Define** intrinsic vs. extrinsic hallucination (anchored to Maynez et al. 2020) and say which one grounding/RAG addresses.
6. **Use** the "autocomplete on steroids — pattern-matcher, not search engine" model to predict where AI will be strong and where it will be confidently wrong.

## Prerequisites

None. This is Session 1. It sets up the mental models that Sessions 2 (vocabulary + cost), 9 (how LLMs work), 12 (confidently wrong), and 14 (limits + jobs) all depend on.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–4 min | **Hook** | "Have you ever remembered something vividly — and been dead wrong?" A show-of-hands opener that plants the whole session. |
| 4–12 min | **Learning by example vs. by rules** | The *data + answers → rules* inversion; one worked example; what it buys and what it costs. |
| 12–22 min | **Human memory is reconstructive** | Memory as reconstruction, not playback; how that manufactures confident false memories. |
| 22–32 min | **The flagship idea** | Hallucination, false memory, and prejudice as one failure mode: pattern-completion outrunning evidence. |
| 32–40 min | **Naming it precisely** | Intrinsic vs. extrinsic hallucination (Maynez et al. 2020); confidence tracks fluency, not truth. |
| 40–45 min | **The mental model** | "Autocomplete on steroids — a pattern-matcher, not a search engine," and what follows from it for the whole series. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md` — the seed question is *"Where have you seen a system be confidently wrong? Was it lying, or reconstructing?"* |

**Is 45 minutes honest?** Yes, if you hold discipline. The temptation is to over-explain transformers — don't; that is Session 9. This session is four ideas, each with one diagram and one example. Cut the interpolation/extrapolation aside (in `content/04`) first if you run long.

## Materials & tools

- Slides: `slides/outline.md` → deck built per `../powerpoint_instructions.md`.
- No notebook, no live network dependency. Optional live demo cue: an LLM confidently fabricating a biography of a made-up person (do this on the presenter's own sandbox account; do **not** put a real colleague's name into a public model). The LLM-safety source deck's own "hallucinated résumé" demo is the model for this — reference it, don't reproduce its slides.
- One reflective exercise instead of a lab (`exercises/lab.md`).

## Source & licence note

| Source | Role in this session | Reuse verdict |
|---|---|---|
| **Maynez et al. 2020**, *On Faithfulness and Factuality in Abstractive Summarization* (ACL Anthology) | Anchors the intrinsic/extrinsic hallucination definitions **on slides** | **SLIDE-SAFE** (CC BY 4.0 — attribute) |
| **AGI Demystified** deck (Ozdemir, O'Reilly) — Memory pillar | The reconstructive-memory ↔ hallucination parallel (idea only) | **LINK-ONLY** — paraphrase, don't reproduce |
| **LLM System Safety and Security** deck (Nield, O'Reilly) | "Autocomplete on steroids / pattern-matcher not search engine" framing | **LINK-ONLY** — paraphrase, attribute the framing |
| Cognitive-science literature (Bartlett; Loftus; Roediger & McDermott) | The false-memory evidence (findings stated in our own words) | **LINK-ONLY** — assign as reading |

The **hallucination-vs-prejudice synthesis is authored** for this course — nothing in the corpus states it directly. Full provenance and licence verdicts: `resources/sources.md`.

> **Honesty note carried from the source (see `content/02`):** the memory ↔ hallucination link is an **analogy**, not a proven identical mechanism. The AGI-deck author says so explicitly. We keep that caveat visible rather than overselling the parallel — it is load-bearing for the course's skeptical voice.
