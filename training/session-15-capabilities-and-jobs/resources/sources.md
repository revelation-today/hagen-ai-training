# Sources & Licences — Session 15

Reuse verdicts govern what may appear on a slide. See `../../powerpoint_instructions.md` §6.

**Summary for this session:** Half A's *ideas* come from two commercial, all-rights-reserved decks and are therefore **paraphrased and redrawn, never reproduced**. Half B — the entire four-role analysis — is **original work authored for this course** and is fully **SLIDE-SAFE**. One external empirical finding is cited with attribution.

---

## 1. Thomas Nield — *Deep Learning for Beginners, Day 3: From Basics to Production with NumPy and TensorFlow*

| | |
|---|---|
| **Author / org** | Thomas Nield · O'Reilly Media (live online training) |
| **Version / date** | Day 3 of 3, 104 slides, 2024-09 |
| **URL** | O'Reilly Learning platform (subscription) |
| **Licence** | All rights reserved (commercial training material) |
| **Reuse verdict** | **LINK-ONLY** |

**Used for (ideas only, all re-authored):** the S-curve argument and the ~80% coverage plateau (`content/03`); selection bias and the Volvo-kangaroo illustration (`content/02` §4a); outliers, the law of truly large numbers, and combinatorial outliers (`content/02` §4b); data rot and infrastructure drift (`content/02` §4c); the proof-of-concept-to-production gap (`content/04`); the data-labelling economy as a cost driver (`content/03` §4).

**Discipline applied:** no slide reproduces source text, figures, or diagram layouts. The S-curve on slide 10 is **redrawn from our own numbers**, not traced from the source figure.

**Onward attributions inside this source, both LINK-ONLY and both paraphrase-only:**
- **The S-curve** is attributed in the source to **Stefan Seltz-Axmacher** (CEO, Starsky Robotics), from his 2020 post-mortem essay on Medium and a related podcast appearance. Attribute the *concept*; never reproduce the text or the figure.
- **Andrew Ng's radiology / data-drift and production-gap remarks** originate in an *IEEE Spectrum* interview ("Andrew Ng X-Rays the AI Hype"). **Paraphrase only — do not quote on any slide.** IEEE content is all-rights-reserved. `content/02` §4c and `content/04` §1 paraphrase; the slide notes instruct the presenter to speak the idea, not the words.
- **François Chollet's** remarks on generalisation, data requirements, and benchmark measure-gaming come from a 2019 *Verge* interview. **Paraphrase only.** Referenced only lightly in this session (`content/03` §5).

---

## 2. Thomas Nield — *LLM System Safety and Security*

| | |
|---|---|
| **Author / org** | Thomas Nield · O'Reilly Media |
| **Version / date** | 122 slides, ~late 2023 |
| **URL** | O'Reilly Learning platform (subscription) |
| **Licence** | All rights reserved |
| **Reuse verdict** | **LINK-ONLY** |

**Used for (ideas only, all re-authored):**
- The decision rule that an LLM application is defensible when the user can **easily verify** the output or when **truth is irrelevant** (`content/01` §1). This is the backbone of the session's bucketing logic.
- The **verification paradox** — the better the model performs, the harder it is for a human to identify the residual failures, and the more verification work there is overall (`content/05` §3b).
- **Job losses from over-automation listed as a hazard outcome**, alongside financial loss, security breaches, and reputational damage (`content/05` §1). This reframing — that over-automation belongs in a risk register — is the source's, and is used here in our own words.
- The observation that requiring a human draft/review step **is the safety control**, not an inconvenience (`content/01` §2).
- The **pedestrian paradox** — when a system has failed to recognise something, how would it recognise that it failed? (`content/02` §3, `content/07` §4).
- The **interpolation vs. extrapolation** framing of hallucination (`content/02` §2).
- The **AI-paralegal exercise debrief** — the prediction that the lawyers end up doing paralegal work themselves while handing first passes to the LLM, and eventually hire a paralegal who uses the tool. This is the single closest thing in the corpus to a per-role job analysis, and it informed the shape of `content/05`–`09` even though none of its text is reused.

**Also noted:** this deck contains **no adversarial-security content** despite its title; that gap is filled in Session 14, not here.

---

## 3. Sinan Ozdemir — *AGI Demystified — Live Session*

| | |
|---|---|
| **Author / org** | Sinan Ozdemir · O'Reilly Media |
| **Version / date** | 212 slides, ~2026 |
| **Licence** | All rights reserved |
| **Reuse verdict** | **LINK-ONLY** |

**Used for one point only:** the honest negative results on reasoning models — that chain-of-thought/reasoning modes sometimes fail to help, and that some reasoning-tuned models hallucinate *more* than their non-reasoning counterparts (`content/02` §2). Paraphrased. This is the evidence for the claim that longer reasoning chains do not convert extrapolation into interpolation.

---

## 4. Pearce, Ahmad, Tan, Dolan-Gavitt & Karri — *Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions*

| | |
|---|---|
| **Authors / org** | H. Pearce, B. Ahmad, B. Tan, B. Dolan-Gavitt, R. Karri (NYU) |
| **Venue / date** | IEEE Symposium on Security and Privacy (S&P) 2022; preprint arXiv:2108.09293 (2021) |
| **URL** | `https://arxiv.org/abs/2108.09293` |
| **Licence** | arXiv preprint — author-posted; IEEE version all-rights-reserved |
| **Reuse verdict** | **CITE-ONLY** — the finding may be stated on a slide **with attribution**; do not reproduce the paper's figures, tables, or text |

**Used for:** the ~39% figure — approximately 39.33% of top-ranked completions in security-relevant scenarios contained a vulnerability. Cited in `content/09` §3a and on slides 16–17.

**Required honesty when presenting it** (both stated in `content/09` §3a and in the slide notes):
1. It was measured on a **specific model generation** in **deliberately security-sensitive scenarios**. It is not "39% of all AI-generated code is vulnerable," and presenting it that way would be misrepresentation.
2. Models have improved since 2021–22.

**Why it still stands:** the *mechanism* is unchanged — nothing in next-token prediction prefers the secure pattern over the common one, and insecure patterns are often more common in public code because they are shorter and appear in tutorials — and generation *volume* has risen sharply. A lower rate over far more code is not obviously fewer vulnerabilities.

---

## 5. Original material authored for this course — **SLIDE-SAFE**

Everything below is written for this session. It may be used freely on slides, in handouts, and in derivative internal material.

| Artefact | Location | Slides |
|---|---|---|
| The four capability families and the capability/ceiling table | `content/01` | 5, 6 |
| The "can't yet vs. can't structurally" distinction | `content/02` §1 | 7 |
| The always/never/all/exactly reflex | `content/02` §3 | 9 |
| The S-curve cost-per-increment table (our own numbers) | `content/03` §2 | 10 |
| "Was the S-curve wrong?" — the two-sided assessment | `content/03` §5 | 11 |
| The gap-item → role-owner mapping table | `content/04` §3 | 12 |
| The four-step turn | `content/04` §5 | 13 |
| **The three-bucket framing plus the fourth bucket ("gets harder")** | `content/05` | 13–17 |
| **The complete four-role task decomposition** | `content/06`–`09` | 14–17 |
| The task → who-owns-it decision flow | `content/10` §1 | 18 |
| Delegate / never-delegate table and the four questions for management | `content/10` §3, §5 | 19 |
| All Mermaid diagrams in this session | throughout | throughout |
| The role self-audit and the catch log | `exercises/lab.md` | — |

**Note on the public labour-market framing.** This session deliberately cites **no** third-party job-displacement forecasts. Such figures are typically headline restatements of task-count studies with methodological caveats the headline dropped, and using them would undermine the session's credibility with an audience trained to check claims (this is the Session 13 discipline applied to ourselves). The automate/augment/gets-harder decomposition is authored here and rests on the capability analysis in `content/01`–`04`, which is itself defensible from the mechanism.

---

## 6. Deliberately not used

| Not used | Why |
|---|---|
| Cisco *Mastering the Fundamentals of AI and ML* | `Cisco Confidential` on every slide — excluded series-wide (see `AI_input.md` §1). |
| Public "X% of jobs will be automated by 20YY" forecasts | Methodologically weak as usually reported; inconsistent with this course's own standard of evidence. |
| Vendor productivity claims for AI coding tools | Vendor material, self-reported, non-neutral. Session 3's discipline: name it as vendor material or don't use it. |
| Any direct quotation of Ng, Chollet, LeCun, or Seltz-Axmacher on a slide | All LINK-ONLY. Paraphrase and attribute the *concept*, per `powerpoint_instructions.md` §6. |

---

## Further reading (LINK-ONLY — assign, do not reproduce)

| Item | Why it's worth the time |
|---|---|
| **Andrew Ng, IEEE Spectrum interview** ("Andrew Ng X-Rays the AI Hype") | The primary statement of the proof-of-concept-to-production gap and the radiology data-drift case. The best 10 minutes of reading behind Half A. |
| **Stefan Seltz-Axmacher**, "The End of Starsky Robotics" (Medium, 2020) | The S-curve argument first-hand, written by someone who paid for the lesson. Short and unusually candid. |
| **François Chollet**, *The Verge* interview (2019) on measuring intelligence | On why optimising a benchmark teaches you nothing about generalisation. Sharpens the capability discussion considerably. |
| **Narayanan & Kapoor**, *AI Snake Oil* (blog and book) | Scientifically grounded assessment of AI capability claims. The most useful ongoing source for anyone who has to evaluate a vendor pitch. |
| **Pearce et al.**, arXiv:2108.09293 | The primary source for the ~39% finding. Read the methodology before citing the number to anyone. |
| **Yann LeCun**, public remarks countering the claim that AI will take everyone's jobs | Useful counterweight, and a reminder that senior figures in the field openly disagree with each other. |
