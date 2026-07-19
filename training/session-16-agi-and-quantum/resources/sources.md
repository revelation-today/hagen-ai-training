# Sources & Licences — Session 16: AGI and an Outlook on Quantum

Reuse verdicts: **SLIDE-SAFE** = may derive slides, prose and figures with attribution · **LINK-ONLY** = reference it, assign it as reading, or show it as a live demo, but **never copy it onto a slide**. When in doubt, LINK-ONLY.

**This session has an unusually thin SLIDE-SAFE list and an unusually long LINK-ONLY list.** That is expected: the AGI half draws on commercial training material and frontier-lab publications, essentially none of which is openly licensed. The response is not to embed it anyway — it is to **write the material ourselves from the ideas**, which is what `content/` does, and to link the primaries.

⚠️ **All benchmark scores, model names and lab positions cited in this session are tagged `[verify at delivery]`.** They were accurate when written and drift within months. Re-check before presenting.

---

## Corpus provenance

**1. "AGI Demystified — Live Session" — Sinan Ozdemir, O'Reilly, ~2026, 212 pages**
- Source file: `C:\Users\hagen\Dropbox\Bibel\AI\raw_input\agidemystifiedlivesession1770656201521.pdf`. Extract: `AI_input.md` §2.5 and the session scratchpad `extract_agi.md`.
- The corpus's **largest and most current** source, and the primary input for the AGI half of this session.
- **Licence: all-rights-reserved commercial training material (O'Reilly live-event deck). Verdict: LINK-ONLY.**
- **How it is used:** as an *organising input only*. The seven-pillar framing, the definition timeline, the honest-negative-results posture and the five-limits structure are used as an **outline**, entirely rewritten in our own words, with our own tables, diagrams, analysis and audience framing. **No slide, figure, layout or phrasing is reproduced.** Where the deck cites a primary source (a paper, a benchmark, a lab post), we cite and link the primary directly — see entries 2–12 below.
- **Quoted material:** the lab quotes in `content/04` are short attributed fragments used for identification of a public position. **On slides they must be paraphrased in our own words and attributed to the speaker, not to this deck.**
- **Assign as:** optional further study for anyone who wants the full treatment (it is good, and it is Ozdemir's to sell).

---

## SLIDE-SAFE — build slides from these

**2. ARC-AGI — Abstraction and Reasoning Corpus (François Chollet)**
- <https://github.com/fchollet/ARC-AGI> · also the ARC Prize materials at <https://arcprize.org>
- **Licence: Apache-2.0 (repository). Verdict: SLIDE-SAFE.** Tasks, grids and derived figures may be shown and screenshotted with attribution. **Tag slide footers `ARC-AGI (Chollet), Apache-2.0`.**
- Used for: `content/03` §3.5 (the benchmark's design properties and status), Slide 12 (a real task as the deck's primary visual, plus the optional live demo), `exercises/lab.md` (our ARC-*style* tasks are original, written in the same format).
- ⚠️ Check the ARC Prize site's own terms separately if you screenshot from there rather than from the repository; the repository's Apache-2.0 is the clean path.

**3. NIST Post-Quantum Cryptography Standards & Project**
- <https://csrc.nist.gov/projects/post-quantum-cryptography> — the standardised algorithm suite and supporting publications.
- **Licence: US Government work — public domain. Verdict: SLIDE-SAFE.** Prose and figures may be reproduced with attribution.
- Used for: `content/06` §6.6, Slide 19 (right column).
- ⚠️ `[verify at delivery]` — confirm the current publication numbers, the standardised algorithm set, and any standards issued since this was written. Do not cite specific document numbers from memory.

**4. Original material — this session's own diagrams, tables, analysis, and code**
- All Mermaid diagrams, all comparison and scorecard tables, the four-question filters, the pillar scorecard, the gap table, the benchmark-status table, and the Python in `exercises/lab.md`.
- **Verdict: SLIDE-SAFE** (ours). One constraint: the accuracy-vs-complexity curve in `content/03` §3.2 is a **teaching schematic, not measured data** and **must be labelled as such on the slide face**, not only in the speaker notes.

---

## LINK-ONLY — reference, assign, or demo; never embed

### The evidence

**5. Apple — "The Illusion of Thinking"** (reasoning-model evaluation on controlled puzzle environments incl. Tower of Hanoi)
- <https://machinelearning.apple.com/research/illusion-of-thinking> (paper PDF linked from there).
- **Licence: all-rights-reserved (corporate research publication). Verdict: LINK-ONLY.** Do not reproduce its figures — use our labelled schematic instead.
- Used for: `content/03` §3.2, Slide 9. ⚠️ **`[verify at delivery]`** — this paper has been actively debated and partially rebutted (output-length objection). Check the current state of the discussion; present the counter-argument as `content/03` does.

**6. OpenAI safety / model evaluation reporting — hallucination rates (PersonQA and similar)**
- <https://openai.com/safety/evaluations-hub/> and current model system cards.
- **Licence: vendor material, all rights reserved. Verdict: LINK-ONLY.** Link the chart; do not screenshot it.
- Used for: `content/03` §3.4, Slide 11. ⚠️ **`[verify at delivery]`** — these figures change with every model release.

**7. Chess world-model probing** — the probing methodology and result described in the AGI source deck (#1), which reports the experiment rather than publishing it as a paper.
- **Verdict: LINK-ONLY** (it travels with #1). Our `content/03` §3.3 describes the *method* — probing internal activations with a small classifier — which is a standard, freely describable technique, and reports the result as attributed to that source.
- Related openly-available background on probing: Clark et al. 2019, *What Does BERT Look At?*, ACL Anthology <https://aclanthology.org/W19-4828/> — **ACL Anthology papers are typically CC BY; verify the individual paper's licence before embedding a figure.**

### Benchmarks and evaluation

**8. Benchmark primaries** — link on the resources slide, do not embed:
- **MMLU** — Hendrycks et al. 2020, <https://arxiv.org/abs/2009.03300>
- **TruthfulQA** — Lin, Hilton & Evans 2022, <https://arxiv.org/abs/2109.07958>
- **Humanity's Last Exam** — <https://lastexam.ai>
- **SWE-bench** — <https://www.swebench.com>
- **GDPval** — <https://openai.com/index/gdpval>
- **Open LLM Leaderboard** — <https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard>
- **Licence: mixed.** arXiv preprints carry per-paper licences (often CC BY, often not — **check each one individually** before reproducing a figure). Leaderboards and vendor pages are all-rights-reserved. **Verdict: LINK-ONLY as a class**, unless you verify an individual paper is CC BY.
- Used for: `content/03` §3.7 (our own table, built from public status — the table itself is ours and is SLIDE-SAFE).

### Lab positions and the AGI debate

**9. Frontier-lab public positions** — link only, paraphrase on slides:
- **Anthropic** — <https://www.anthropic.com/news> (Amodei's essays and interviews)
- **Meta / FAIR** — LeCun, *A Path Towards Autonomous Machine Intelligence*, <https://openreview.net/pdf?id=BZ5a1r-kVsf>; V-JEPA world-model work at <https://ai.meta.com/blog/>
- **Google DeepMind** — world-model research, <https://deepmind.google/discover/blog/>
- **Mistral, DeepSeek, AI2** — public statements and blogs; AI2 at <https://allenai.org>
- **Licence: all-rights-reserved corporate publications. Verdict: LINK-ONLY.**
- Used for: `content/04`, Slide 13. **Quotes must be paraphrased on slides and attributed to the speaker.** ⚠️ `[verify at delivery]` — leadership and positions change.

**10. Philosophical and cognitive-science background** — named concepts, no reproduction needed:
- Turing (1950), *Computing Machinery and Intelligence*; Searle (1980), the Chinese Room; Dennett on the narrative self; Clark & Chalmers (1998), *The Extended Mind*; Chomsky on innate structure; Damasio, *Descartes' Error*; the Churchlands on eliminative materialism and neural computation.
- **Verdict: the *ideas* are freely describable and are described in our own words in `content/02`. The *texts* are copyrighted — LINK-ONLY.** Do not quote at length.

**11. Legg & Hutter / Goertzel — formal AGI definitions**
- Legg & Hutter, *A Collection of Definitions of Intelligence*, <https://arxiv.org/abs/0706.3639> — **check the arXiv licence on this specific paper before reproducing anything**; the definitions themselves are short factual statements we restate in our own words.
- **Verdict: LINK-ONLY** for the text; our timeline table is original.

### Quantum

**12. Quantum computing background** — the physics in `content/06` is standard textbook material, authored here from general knowledge and stated at a deliberately high level. Recommended further reading (all **LINK-ONLY** unless individually verified):
- **Nielsen & Chuang**, *Quantum Computation and Quantum Information* — the standard graduate text. Copyrighted.
- **Preskill (2018)**, *Quantum Computing in the NISQ Era and Beyond*, <https://arxiv.org/abs/1801.00862> — the paper that named the NISQ era. **Check its arXiv licence** (many Preskill papers are CC BY — verify before reproducing).
- Vendor quantum roadmaps (IBM, Google, IonQ, Rigetti and others) — **all vendor material; LINK-ONLY, and read them with the §6.7 filter applied.** They are roadmaps, not results.
- **On dequantisation:** Ewin Tang's work on classical algorithms matching proposed quantum ML speedups — a good entry point for why QML claims deserve scrutiny. Search current literature; verify licences individually.
- **On barren plateaus:** the variational-quantum-circuit training-difficulty literature. Same caveat.

---

## Quick verdict table

| # | Source | Licence | Verdict |
|---|---|---|---|
| 1 | AGI Demystified deck (Ozdemir, O'Reilly) | all rights reserved | **LINK-ONLY** — outline input only; nothing reproduced |
| 2 | **ARC-AGI** (`github.com/fchollet/ARC-AGI`) | **Apache-2.0** | **SLIDE-SAFE** — tasks & figures, with attribution |
| 3 | **NIST post-quantum cryptography** | **US-gov public domain** | **SLIDE-SAFE** |
| 4 | **This session's own diagrams, tables, code** | ours | **SLIDE-SAFE** |
| 5 | Apple, *The Illusion of Thinking* | all rights reserved | **LINK-ONLY** |
| 6 | OpenAI evaluation / hallucination data | vendor, ARR | **LINK-ONLY** |
| 7 | Chess world-model probe (via #1) | via #1 | **LINK-ONLY** |
| 8 | Benchmark primaries (MMLU, TruthfulQA, HLE, SWE-bench, GDPval) | mixed — check per paper | **LINK-ONLY** as a class |
| 9 | Frontier-lab blogs, papers, quotes | ARR | **LINK-ONLY** — paraphrase on slides |
| 10 | Philosophy / cognitive-science texts | copyrighted | **LINK-ONLY** — ideas described in our own words |
| 11 | Legg & Hutter definitions paper | check arXiv licence | **LINK-ONLY** |
| 12 | Quantum background (Nielsen & Chuang, Preskill, vendor roadmaps) | mixed | **LINK-ONLY** unless individually verified |

---

## Further reading (LINK-ONLY, high quality — assign, don't embed)

**For the AGI half**
- **ARC-AGI / ARC Prize** — <https://arcprize.org> and <https://github.com/fchollet/ARC-AGI>. **Go and try ten tasks.** Twenty minutes of this beats any amount of reading about generalisation, and it is the single best follow-up in this session.
- **Chollet (2019), *On the Measure of Intelligence*** — <https://arxiv.org/abs/1911.01547>. The paper behind the skill-vs-intelligence distinction and behind ARC. The most intellectually serious document in this reading list.
- **LeCun, *A Path Towards Autonomous Machine Intelligence*** — <https://openreview.net/pdf?id=BZ5a1r-kVsf>. The world-model argument, from its strongest advocate.
- **Apple, *The Illusion of Thinking*** — plus at least one published rebuttal. Reading both is the exercise.
- **Narayanan & Kapoor, *AI Snake Oil*** — book and newsletter. The most consistently useful hype-correction source available, and closest in temperament to this series' voice.
- **Ozdemir, *AGI Demystified*** (O'Reilly live session, source #1) — for anyone who wants the full seven-pillar treatment from the original.

**For the quantum half**
- **Preskill (2018), *Quantum Computing in the NISQ Era and Beyond*** — <https://arxiv.org/abs/1801.00862>. If you read one quantum paper, read this one; it is the honest framing of where the field is.
- **NIST post-quantum cryptography project** — <https://csrc.nist.gov/projects/post-quantum-cryptography>. The actionable material.
- **Your national cybersecurity agency's PQC migration guidance** (e.g. BSI in Germany, NCSC in the UK, CISA/NSA in the US) — these are practical migration playbooks, generally public-sector and often freely reusable. **Verify the licence per document**, but these are the closest thing to a checklist for the crypto-agility work described in `content/06` §6.6.

---

## A note on this session's licence posture

This session discusses a great deal of material it cannot reproduce. That constraint shaped the deliverable, and the result is better for it: because nothing could be lifted, **every table, diagram, framing and argument in `content/` had to be written from the underlying ideas.** The two genuinely SLIDE-SAFE assets — **ARC-AGI** and **NIST's PQC standards** — carry the two most important visuals in the deck, which is a fortunate accident worth exploiting. Use the ARC task on Slide 12 and the NIST material on Slide 19 generously; everything else is a link.
