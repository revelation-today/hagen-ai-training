# Sources — Session 1

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / BSD / public-domain / standards body. May derive slide and content text/figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NC / ND / vendor / internal. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY. The **hallucination-vs-prejudice synthesis** in `content/03` is **authored for this course** — no source states it directly, so it carries no third-party licence constraint.

---

## Slide-safe (embeddable with attribution)

**#1 — Maynez, Narayan, Bohnet & McDonald (2020), *On Faithfulness and Factuality in Abstractive Summarization*.**
- Venue: Proceedings of ACL 2020, via the **ACL Anthology**.
- URL: `https://aclanthology.org/2020.acl-main.173/`  · arXiv: `https://arxiv.org/abs/2005.00661`
- Licence: **CC BY 4.0** (ACL Anthology standard for ACL 2020 proceedings — attribution required; derivation permitted).
- Used for: the **intrinsic vs. extrinsic hallucination** definitions in `content/04` and slide 13. *Intrinsic* = output contradicts the source; *extrinsic* = output cannot be verified from the source.
- **Reuse verdict: SLIDE-SAFE.** Attribute on-slide as "Definitions: Maynez et al. 2020 (CC BY 4.0)."
- *Verify at delivery:* confirm the CC BY 4.0 notice on the Anthology page still displays (Anthology per-paper licences are stable, but check before publishing the deck externally).

---

## Link-only (reference / assign / live-demo — never embed)

**#2 — Ozdemir, S. *AGI Demystified — Live Session* (O'Reilly).** Source deck, Memory pillar (reconstructive memory ↔ hallucination), pp. 24–27; hallucination taxonomy p. 92.
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- Used for: the **memory-reconstruction ↔ hallucination analogy** (`content/02`), *paraphrased in our own words*. The author's own **honesty caveat** — that this is a striking *parallel*, not a claim of identical mechanism — is carried through `content/02`, the README, and slide 10. The "reasoning models can hallucinate more on recall than earlier models" aside is from this deck (pp. 27) and is **currency-sensitive** — verify at delivery if used, and keep it link-only.

**#3 — Nield, T. *LLM System Safety and Security* (O'Reilly).** Source deck, "Understanding LLMs" four-step progression and the summary claim (slides 6–9).
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- Used for: the **"autocomplete on steroids… a pattern-spotting and matching engine, not a search engine looking up facts"** framing (`content/04`, slide 15), and the **interpolation vs. extrapolation** grounding of the hallucination mechanism. Delivered in our own words; the phrase "autocomplete on steroids" is common parlance. Attribute the *framing* verbally; do not reproduce the deck's slides.

**#4 — Bartlett, F. C. (1932), *Remembering: A Study in Experimental and Social Psychology*.** The founding work on **reconstructive** (schema-based) memory.
- Licence: all-rights-reserved (Cambridge University Press). **LINK-ONLY** — assign as background reading; state the concept in our own words.
- Used for: the "memory is a rebuild, not a recording" foundation in `content/02`.

**#5 — Loftus, E. F. and colleagues — the misinformation effect and false-memory implantation.** Representative works: Loftus & Palmer (1974), *Reconstruction of Automobile Destruction* (the "smashed" vs. "hit" study); Loftus & Pickrell (1995), *The Formation of False Memories* ("lost in the mall").
- Licence: all-rights-reserved (journal articles). **LINK-ONLY** — findings paraphrased; do not reproduce figures.
- Used for: the "confident false memories are easy to seed" evidence in `content/02` / slide 8.
- *Verify at delivery:* if you put a specific statistic on a slide, pull the exact figure from the primary paper, not a secondhand summary — a technical audience will check.

**#6 — Roediger, H. L. & McDermott, K. B. (1995), *Creating False Memories: Remembering Words Not Presented in Lists* (the DRM paradigm).**
- Licence: all-rights-reserved (Journal of Experimental Psychology). **LINK-ONLY** — findings paraphrased.
- Used for: the word-list false-memory example ("people confidently 'remember' *sleep*") in `content/02` / slide 8.

**#7 — Interpolation vs. extrapolation in neural models — arXiv:2311.00388.**
- URL: `https://arxiv.org/abs/2311.00388`
- Licence: arXiv non-exclusive license (assume all-rights-reserved unless a CC notice is shown). **LINK-ONLY.**
- Used for: the optional aside in `content/04` grounding hallucination as "extrapolation into sparse, brittle space."

**#8 — Financial Times, *Generative AI exists because of the transformer* (visual explainer).**
- URL: `https://ig.ft.com/generative-ai/`
- Licence: all-rights-reserved (FT). **LINK-ONLY** — assign as reading; do not reproduce.
- Used for: optional pre-reading on next-token generation (the mechanism behind slide 15).

---

## Further reading (LINK-ONLY, high quality — assign, don't slide)

| Topic | Suggestion | Why |
|---|---|---|
| Reconstructive memory, plainly | Elizabeth Loftus's TED talk *How reliable is your memory?* | The most accessible entry to the false-memory evidence for a non-specialist audience. |
| How LLMs actually generate text | FT *Generative AI* explainer (#8) | A visual, non-mathematical account of next-token prediction; sets up Session 9. |
| The skeptical voice of the series | *AI Snake Oil* (Narayanan & Kapoor) | "Memorization is a spectrum"; calibrated takes on what AI can and can't do. Recurs in Sessions 13–14. |
| The hallucination taxonomy at the source | Maynez et al. 2020 (#1) | The one slide-safe primary source in this session — read it once to own the intrinsic/extrinsic distinction. |

---

### Correction carried into this session (per `../../../AI_input.md` §6)

The source Deep Learning course's colour example contains a **contradiction** — one slide says a model output ≥ 0.5 means *dark* background, another says *light* (error register #1). We do **not** reproduce that; where the colour example appears (`content/01`, slide 6) it is used only to make the point that *once a rule lives inside a model as numbers, a human can no longer glance at it and be sure which way round it goes.* The contradiction is named as evidence of opacity, never taught as fact.
