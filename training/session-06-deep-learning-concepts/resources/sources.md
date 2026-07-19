# Sources & Licences — Session 6: Deep Learning, Conceptually

Every source this session draws on, with its licence and a reuse verdict.

**Verdicts:** **SLIDE-SAFE** = permissive / CC-BY / BSD / public-domain — may derive slide text and figures **with attribution**. **LINK-ONLY** = all-rights-reserved, NC or ND — reference it, assign it as reading, or show it as a live demo; **never copy it onto a slide**. Per `../../_TEMPLATE/SESSION_STRUCTURE.md` §4, when a licence is uncertain the verdict is **LINK-ONLY** by default.

**The short version for this session:** the *substance* of Session 6 (neuron → layers → forward pass → activations → training → overfitting) is re-authored in our own words, with our own numbers and our own Mermaid diagrams. The source decks are LINK-ONLY and nothing is reproduced from them, so no slide in this deck carries a source-derived figure. The only external artefacts named in the deck (3Blue1Brown, Desmos) are pre-reading and live demo respectively.

---

## 1. 3Blue1Brown — *Neural Networks* video series (chapters 1–4)

- **Author/org:** Grant Sanderson (3Blue1Brown).
- **URL:** <https://www.3blue1brown.com/topics/neural-networks> · playlist: <https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi>
- **Date/version:** chapters 1–4 published 2017; series extended 2024 (later chapters cover transformers and are **not** assigned here).
- **Licence:** all rights reserved. The channel offers no open licence for the videos or their frames.
- **Verdict: LINK-ONLY** — assign chapters 1–4 as **pre-reading/pre-watching**; never embed a frame, still, or animation on a slide.
- **Used by:** `README.md` (pre-reading), `exercises/lab.md` (reflective exercise), `slides/outline.md` resources slide. Its visual intuition for "neuron as a weighted sum" and "layers as composed transformations" is the same picture we draw independently.

## 2. *Deep Learning for Beginners — Day 1* (internal source deck)

- **Author/org:** Thomas Nield, published by O'Reilly. Held internally as course material (`oreillydeeplearningforbeginnersday1…`, 78 pp.).
- **URL:** no public URL — internal corpus copy; see `../../AI_input.md` §1 (deck #1).
- **Date/version:** 2024-08.
- **Licence:** commercial training material, all rights reserved.
- **Verdict: LINK-ONLY** — internal reference and background only. Every concept taken from it is re-pitched and re-written; no slide, table, figure, or code block is reproduced.
- **Used by:** the conceptual spine of `content/01`–`content/04` (neuron anatomy, layers, forward propagation, activation functions) and the light/dark-text running example.
- **Corrections applied (do not repeat the source's errors — see `../../AI_input.md` §6):**
  - **#1 — the ≥0.5 threshold contradiction.** Day 1 p.26 says output ≥ .5 → **DARK**; p.35 says ≥ .5 → **light**. These cannot both hold. We resolve in favour of **≥0.5 → DARK**, because the deck defines the output as *the probability of predicting dark font*, which p.26 matches. Held consistently in `content/03`, `content/99`, `exercises/quiz.md` (Q4) and `slides/outline.md` (slides 11 and 14).
  - **#13 — "deep" vs. the running example.** Day 1 p.16 defines deep learning as more than one hidden layer, while the course's own 3→3→1 example has exactly one. We state this openly in `content/02` rather than glossing over it.
  - **#2 — weight initialisation.** The source says weights start in −1…1 but uses `np.random.rand`, which yields 0…1. Not material to this session (we never show initialisation code), noted here so it is not carried forward into Session 7.
  - **MSE with a sigmoid/softmax output.** The source uses mean-squared error throughout for consistency; cross-entropy is the conventional pairing. Flagged honestly in the footnote in `content/04`, not presented as best practice.

## 3. *Deep Learning for Beginners — Day 2* (internal source deck)

- **Author/org:** Thomas Nield, published by O'Reilly (`oreillydeeplearningforbeginnersday2…`, 84 pp.).
- **URL:** no public URL — internal corpus copy; see `../../AI_input.md` §1 (deck #2).
- **Date/version:** 2024-08.
- **Licence:** commercial training material, all rights reserved.
- **Verdict: LINK-ONLY** — internal reference and background only; nothing reproduced.
- **Used by:** `content/05` (loss, gradient descent, learning rate, backpropagation, stochastic/mini-batch sampling). Day 2 derives all of this from scratch with calculus; we deliberately teach it **by intuition** instead, and the flashlight-in-the-mountains framing is re-authored in our own words (`slides/outline.md` slide 15).
- **Correction applied:** `../../AI_input.md` §6 **#4** — Day 2 p.6 prints a rise-over-run slope of 4.41 where the correct value is **4.1**. That specific figure is not reused in this session.

## 4. TensorFlow / Keras official documentation

- **Author/org:** Google / the Keras team.
- **URL:** <https://www.tensorflow.org/api_docs/python/tf/keras> · <https://keras.io/api/layers/core_layers/dense/>
- **Date/version:** TensorFlow 2.x / Keras 3.x (current at 2026-07).
- **Licence:** Apache-2.0 (code and API documentation); code samples Apache-2.0, prose under CC-BY-4.0 on tensorflow.org.
- **Verdict: SLIDE-SAFE** — API names, signatures and idiomatic snippets may go on a slide with attribution.
- **Used by:** the `Dense` = "fully connected" naming in `content/02`, the forward pointer to the ~5-line Keras build in `content/00`, `content/03`, `content/99` and `exercises/discussion.md`. Session 6 shows no Keras code itself; Session 7 does.

## 5. NumPy documentation and API

- **Author/org:** NumPy developers.
- **URL:** <https://numpy.org/doc/stable/>
- **Date/version:** NumPy 2.x (current at 2026-07).
- **Licence:** BSD-3-Clause.
- **Verdict: SLIDE-SAFE** — API usage and idioms may be reproduced with attribution.
- **Used by:** the single Python block in `content/03` (matrix form of the forward pass, `np.maximum` / `np.exp`). The snippet itself is original; only the API is NumPy's.

> **scikit-learn** (BSD-3-Clause, SLIDE-SAFE) is **not cited in this session** — Session 6 shows no scikit-learn code. It carries the load in Sessions 4, 5 and 8; see those sessions' `resources/sources.md`. Listed here only so the absence is deliberate rather than an oversight.

## 6. Desmos activation-function grapher (optional live demo)

- **Author/org:** graph authored by Thomas Nield, hosted on Desmos.
- **URL:** <https://www.desmos.com/calculator/jwjn5rwfy6>
- **Date/version:** accompanies the 2024 source course; link live as of the corpus extraction (`../../AI_input.md` §7, Interactive tools).
- **Licence:** hosted under the Desmos Terms of Service; the graph itself carries no open licence. **Uncertain → safe default applies.**
- **Verdict: LINK-ONLY (live demo)** — open it in a browser for ~60 seconds during the activation-functions segment. **Do not screenshot it onto a slide.** Our own activation comparison table (`content/04`) is the slide asset.
- **Used by:** `content/04` ("Try it live"), `README.md` materials, `slides/outline.md` slide 13.

## 7. `../../AI_input.md` — corpus analysis and error register

- **Author/org:** this project's own corpus analysis of the seven source decks.
- **URL:** internal, `output/AI_input.md` (§1 deck inventory, §6 error register, §7 asset index).
- **Date/version:** 2026 working document.
- **Licence:** internal Qualcomm work product — our own writing about the sources, not the sources themselves.
- **Verdict: SLIDE-SAFE** (internally) — but note that quoting it does not launder the licence of the LINK-ONLY decks it describes.
- **Used by:** every correction listed under sources #2 and #3 above.

## 8. Choromanska et al., *The Loss Surface of Multilayer Networks*

- **Author/org:** Anna Choromanska, Mikael Henaff, Michael Mathieu, Gérard Ben Arous, Yann LeCun.
- **URL:** <https://arxiv.org/abs/1412.0233>
- **Date/version:** arXiv, 2014 (v3); AISTATS 2015.
- **Licence:** arXiv non-exclusive distribution licence — **not** an open-content licence unless the authors selected one; treat as uncertain.
- **Verdict: LINK-ONLY** — cite the finding in words (as we do), do not reproduce its figures or text.
- **Used by:** the claim in `content/05` ("Step 5 — the landscape is bumpy") and `exercises/discussion.md` that in large networks most local minima are near-equivalent in quality, so gradient descent does not need the *global* minimum to work. Stated as an established result in our own words; no figure reused.

---

## Quick verdict table

| # | Source | Licence | Verdict |
|---|---|---|---|
| 1 | 3Blue1Brown, *Neural Networks* ch. 1–4 | all rights reserved | **LINK-ONLY** (pre-reading) |
| 2 | Nield, *DL for Beginners* Day 1 (internal deck) | commercial, ARR | **LINK-ONLY** (internal reference) |
| 3 | Nield, *DL for Beginners* Day 2 (internal deck) | commercial, ARR | **LINK-ONLY** (internal reference) |
| 4 | TensorFlow / Keras docs | Apache-2.0 / CC-BY-4.0 | **SLIDE-SAFE** |
| 5 | NumPy docs & API | BSD-3-Clause | **SLIDE-SAFE** |
| 6 | Desmos activation grapher | Desmos ToS, no open licence | **LINK-ONLY** (live demo) |
| 7 | `AI_input.md` corpus analysis | internal work product | **SLIDE-SAFE** (internal) |
| 8 | Choromanska et al. 2014 (arXiv 1412.0233) | arXiv licence, uncertain | **LINK-ONLY** |
| — | scikit-learn | BSD-3-Clause | SLIDE-SAFE — *not cited this session* |

---

## Further reading

High-quality **LINK-ONLY** material. Assign it, link it, demo it — never copy it onto a slide.

1. **3Blue1Brown, *Neural Networks* chapters 1–4** — <https://www.3blue1brown.com/topics/neural-networks>. The single best visual companion to this session; ~60 minutes total. All rights reserved. **Assign as pre-reading before Session 7.**
2. **Michael Nielsen, *Neural Networks and Deep Learning*** (free online book) — <http://neuralnetworksanddeeplearning.com/>. Chapters 1–2 cover exactly our arc — a neuron, a network, and backpropagation — one level deeper, with the calculus we skipped. **Licensed CC-BY-NC 3.0; the NC clause makes it LINK-ONLY for internal corporate training.** Excellent as recommended self-study.
3. **Aurélien Géron, *Hands-On Machine Learning*, ch. 10 ("Introduction to Artificial Neural Networks with Keras")** — book text all rights reserved (O'Reilly) → **LINK-ONLY**. Note that the companion notebooks at <https://github.com/ageron/handson-ml3> are Apache-2.0 and therefore SLIDE-SAFE for *code* reuse; the book's prose and figures are not.
4. **Choromanska et al., *The Loss Surface of Multilayer Networks*** — <https://arxiv.org/abs/1412.0233>. For anyone who asks the sharp version of "but doesn't gradient descent get stuck?" after slide 15.
5. **Desmos activation-function grapher** — <https://www.desmos.com/calculator/jwjn5rwfy6>. Live demo only.
6. **Nield, *Deep Learning for Beginners* Days 1–3 (O'Reilly)** — the full from-scratch NumPy derivation this session deliberately omits. Internal copies only; **LINK-ONLY**. Point curious participants at the commercial course rather than circulating the decks.
