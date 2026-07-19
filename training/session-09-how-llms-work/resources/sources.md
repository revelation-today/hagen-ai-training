# Sources — Session 9

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / BSD / public-domain / standards body. May derive slide and content text and figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NonCommercial / NoDerivatives / vendor / internal. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY.

> ## ⚠️ Read this first — this session has the sharpest licence traps in the series
>
> Three of the four best transformer explanations in the world are **unusable** in this deck:
>
> 1. **Jay Alammar's *The Illustrated Transformer* is CC BY-NC-SA 4.0.** The **NonCommercial** clause covers internal corporate training. His Q/K/V diagrams are the ones every deck-builder instinctively reaches for. **Do not redraw them, do not screenshot them, do not "reproduce the idea in the same layout."** Link it as pre-reading. This is the single most likely licence mistake in the whole course.
> 2. **3Blue1Brown is all-rights-reserved.** His FAQ permits clips under ~60 seconds with added commentary and no re-upload; it does not permit lifting figures into a deck. Assign the videos.
> 3. **The `Cisco Confidential` deck is EXCLUDED** from this course entirely (`output/AI_input.md` §1). It was the corpus's only deep transformer treatment. **Nothing in this session derives from it, and it is not listed below as a source.** See the note at the end of this file for why the Snow White example is nonetheless safe.
>
> Everything in this session's `content/` and `slides/` — every diagram, table, worked number, and code block — is **authored for this course** and carries no third-party constraint.

---

## Slide-safe (embeddable with attribution)

**#1 — Transformer Explainer (Cho, Kim, Wang, Chau et al.; Georgia Tech, Polo Club of Data Science).**
- URL: `https://poloclub.github.io/transformer-explainer/` · Repo: `https://github.com/poloclub/transformer-explainer`
- Licence: **MIT.**
- What it is: an interactive browser tool running **GPT-2 small (124M)** client-side via ONNX. Type text; see tokenisation, attention maps, next-token probabilities, and a live **temperature slider**.
- Used for: **the session's primary live demo** (slides 12 and 16) and as the reference model whose public configuration backs the parameter breakdown in `content/04`.
- **Reuse verdict: SLIDE-SAFE.** Screenshots are permitted. Attribute on-slide as "Transformer Explainer, Georgia Tech (MIT)."
- *Verify at delivery:* confirm the tool still loads and the temperature slider behaves as described. **Capture fallback screenshots the day before** — the deck must present with no network.

**#2 — Hugging Face LLM Course.**
- URL: `https://huggingface.co/learn/llm-course/chapter1/1`
- Licence: **Apache-2.0.**
- Used for: the BPE / subword-tokenisation framing in `content/01` and the encoder / decoder / encoder-decoder taxonomy in `content/04`. Delivered in our own words and tables; the licence would permit closer derivation.
- **Reuse verdict: SLIDE-SAFE.** Attribute "after the Hugging Face LLM Course (Apache-2.0)."
- Also the recommended **follow-up self-study path** for anyone who wants the full treatment.

**#3 — Raschka, S., *Build a Large Language Model (From Scratch)* — companion repository.**
- Repo: `https://github.com/rasbt/LLMs-from-scratch` — **Apache-2.0.**
- Used for: cross-checking the attention arithmetic and the block structure in `content/03`–`04` against a rigorous reference implementation. Bonus directories implement modern architectures (Llama 3.2, Qwen3, Gemma 3, MoE, DPO) from scratch — the best openly-licensed route to the post-2024 architecture material this session only names.
- **Reuse verdict: SLIDE-SAFE** (repo code and figures, with attribution).
- ⚠️ The **book itself is paid** and the author's Substack posts are **all-rights-reserved** — the Apache licence covers the repository only. Do not derive from the blog prose.

**#4 — Karpathy, A. — nanoGPT, build-nanoGPT, and microgpt.**
- microgpt (2026-02): `https://karpathy.github.io/2026/02/12/microgpt/` · nanoGPT: `https://github.com/karpathy/nanoGPT` · Course page: `https://karpathy.ai/zero-to-hero.html`
- Licence: **MIT** (code repositories). Videos are free on YouTube but are **not** MIT.
- Used for: the optional "see it actually run" demo and the lab's extension exercise. **microgpt** is a complete GPT — dataset, tokenizer, autograd, GPT-2-style network, Adam, training, inference — in about **200 lines of dependency-free Python**, readable top to bottom in a session.
- **Reuse verdict: SLIDE-SAFE (code, MIT). Videos LINK-ONLY.**

**#5 — Tiktokenizer.**
- URL: `https://tiktokenizer.vercel.app/` · Repo: `https://github.com/dqbd/tiktokenizer` — **MIT.**
- Used for: the tokenisation live demo (slide 6). Paste text, see colour-coded subword boundaries and counts across encodings; runs entirely client-side, no login.
- **Reuse verdict: SLIDE-SAFE** (screenshots permitted, attribute). Backups if it is down: `platform.openai.com/tokenizer`, or the `tiktoken` code in `exercises/lab.md`.

---

## Cite-as-a-claim (findings usable; figures are not)

**#6 — Chroma Research, "Context Rot: How Increasing Input Tokens Impacts LLM Performance" (Hong, Troynikov, Huber; July 2025).**
- URL: `https://www.trychroma.com/research/context-rot` · Code and data: `https://github.com/chroma-core/context-rot`
- Findings used in `content/06`: performance degrades as input length grows **even when retrieval is effectively perfect**; distractors become more damaging at length; context is used non-uniformly across 18 models tested.
- **Reuse verdict: cite the findings in our own words. Do NOT reproduce their figures.** Our U-curve chart is an **original schematic** and is labelled as such on the slide.
- ⚠️ *Verify before delivery:* confirm the top-level LICENSE file on `chroma-core/context-rot` if you ever intend to reuse an actual figure. We currently do not.

**#7 — Liu, N. F. et al., "Lost in the Middle: How Language Models Use Long Contexts" (TACL 2023).**
- arXiv: `https://arxiv.org/abs/2307.03172`
- Finding used in `content/06`: accuracy is **U-shaped** in the position of the relevant document within a long context — best at the beginning and the end, measurably worse in the middle.
- **Reuse verdict: cite as a claim.** Assume all-rights-reserved unless a CC notice is shown on the version you check; do not reproduce figures.

**#8 — Vaswani, A. et al., "Attention Is All You Need" (NeurIPS 2017).**
- arXiv: `https://arxiv.org/abs/1706.03762`
- The origin of the architecture and the `softmax(QKᵀ/√d_k)V` formulation used throughout `content/03`.
- **Reuse verdict: cite the equation and the paper** (a formula is not a copyrightable expression, and our explanation of each term is original). **Do not reproduce the paper's figures.**

**#9 — Additional long-context evidence, for anyone challenged on the claim.**
- **NoLiMa** — benchmark showing large drops when literal lexical cues are removed at length.
- **"Context Length Alone Hurts LLM Performance Despite Perfect Retrieval"** — `https://arxiv.org/abs/2510.05381` (Oct 2025).
- **Reuse verdict: cite as claims only.** *Verify at delivery — this is an active area and the numbers move.*

**#10 — Tokenisation and symbolic reasoning.**
- `https://arxiv.org/abs/2505.14178`
- Background for the `content/01` claim that fragmenting numbers into arbitrary subword pieces contributes to unreliable arithmetic.
- **Reuse verdict: cite as a claim.**

---

## Link-only (assign, demo, or reference — never embed)

**#11 — Alammar, J., *The Illustrated Transformer*.**
- URL: `https://jalammar.github.io/illustrated-transformer/`
- Licence: **CC BY-NC-SA 4.0 — NonCommercial.**
- **Reuse verdict: LINK-ONLY. Assign as pre-reading.** Still the clearest static walkthrough of Q/K/V in existence, which is exactly why the temptation to reuse it is dangerous. **Internal corporate training is not a NonCommercial use.** Do not redraw, screenshot, or reproduce the layout of his diagrams. Our `content/03` figures were authored independently.
- Note: pre-dates KV-cache, GQA, and RoPE. See #13 for the modern layer.

**#12 — 3Blue1Brown (Grant Sanderson), Deep Learning Ch. 5 *Transformers* and Ch. 6 *Attention in transformers*.**
- URLs: `https://www.3blue1brown.com/lessons/gpt` · `https://www.3blue1brown.com/lessons/attention`
- Licence: **all rights reserved.** The FAQ permits clips under ~60 seconds with added commentary and prohibits re-upload; broader use requires written permission.
- **Reuse verdict: LINK-ONLY. Assign as pre-reading** (~50 min for both). The best geometric intuition available for what attention does in embedding space.

**#13 — DeepLearning.AI, "How Transformer LLMs Work" (Alammar & Grootendorst, 2025).**
- URL: `https://www.deeplearning.ai/short-courses/how-transformer-llms-work/`
- Licence: platform terms of service. **LINK-ONLY.**
- Why it is listed: this is the *current* Alammar, and it covers exactly the modern material this session names but does not teach — **KV cache, multi-query and grouped-query attention, sparse attention, mixture-of-experts**. Best single free follow-up for anyone who wants the 2026 architecture layer.

**#14 — Financial Times, "Generative AI exists because of the transformer: This is how it works."**
- URL: `https://ig.ft.com/generative-ai/` (Sept 2023)
- Licence: **FT copyright. LINK-ONLY.**
- Best narrative/visual framing for a mixed-seniority audience; good optional pre-reading for non-developers. *Verify metering at delivery.*

**#15 — bbycroft, LLM Visualization (llm-viz).**
- URL: `https://bbycroft.net/llm` · Repo: `https://github.com/bbycroft/llm-viz`
- A 3-D interactive walkthrough of a working GPT — unmatched for conveying physical scale and data flow through every matmul.
- **Reuse verdict: LIVE DEMO ONLY.** MIT is commonly cited but the top-level licence was not confirmed during research. ⚠️ **Confirm the LICENSE file before reusing any asset.** Showing the live site is the intended use and is fine.

**#16 — AnimatedLLM (Kasner & Dušek, TeachNLP @ EACL 2026).**
- URL: `https://animatedllm.github.io` · Paper: `https://arxiv.org/abs/2601.04213`
- Purpose-built teaching animation of the full forward pass. **Licence unstated — LINK / LIVE-DEMO ONLY until confirmed.**

**#17 — Nield, T., *LLM System Safety and Security* (O'Reilly). Source deck.**
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- Used for: continuity only — the "autocomplete on steroids… a pattern-spotting and matching engine, not a search engine looking up facts" framing that Session 1 established and `content/07` pays off mechanically. The phrase is common parlance; attribute the framing verbally and reproduce nothing.

---

## Excluded source

**Barton, R. & Henry, J., *Mastering the Fundamentals of AI and ML*.** Every slide carries a **`Cisco Confidential`** classification banner. Using another company's confidentiality-marked internal material to train Qualcomm employees is not defensible. **EXCLUDED** (`output/AI_input.md` §1). It was the corpus's only deep transformer treatment — which is why this session is largely authored, and why sources #1–#5 exist.

### The Snow White example — why it is safe to use

A version of the *"Who is Snow White?"* / *"Why is snow white?"* minimal pair appears in the excluded deck. Three points, stated so the decision is auditable:

1. **The pair is a natural English ambiguity**, not an authored work — the same class of wordplay as "time flies like an arrow." A short factual phrase of this kind is not itself protectable, and it is the kind of example that arises independently.
2. **Nothing was taken.** We do not have, and did not consult, that deck's rendering of it. The framing (`content/03`), the toy 4-D vector space, the projection matrices, the computed attention weights, the two-layer divergence demonstration, all diagrams, and all code are **written from scratch for this course** and were produced by running our own code.
3. **We go further than the idea requires.** The excluded deck's treatment, as catalogued, asserts that attention distinguishes the two. Ours *computes* the difference, shows the divergence compounding across layers, and includes the honesty caveat that attention weights are routing rather than reasoning.

If the excluded deck were ever licensed, this session would not need changing. It is not cited as a source because it is not one.

---

## Further reading (the good LINK-ONLY material, in the order to work through it)

1. **3Blue1Brown Ch. 5 and Ch. 6** (#12) — watch first. ~50 min. The geometric intuition.
2. **Jay Alammar, *The Illustrated Transformer*** (#11) — read second. ~35 min. The static walkthrough.
3. **FT explainer** (#14) — ~15 min. The narrative version; good to send to non-technical colleagues.
4. **DeepLearning.AI, "How Transformer LLMs Work"** (#13) — 1h44m. The modern layer: KV cache, GQA, sparse attention, MoE.
5. **Hugging Face LLM Course** (#2, Apache-2.0) — the structured full course, and the one you may also build from.
6. **Raschka, *LLMs-from-scratch*** (#3, Apache-2.0) — for engineers who want to implement it in PyTorch, including modern architectures.
7. **Karpathy microgpt** (#4, MIT) — 200 lines, one sitting. Read it when you want the "so that's *all* it is" moment.

## Verify-before-delivery list

- [ ] Transformer Explainer and Tiktokenizer both load; **fallback screenshots captured**.
- [ ] `tiktoken` still installs in Colab; the encoding name in `content/01` and `exercises/lab.md` matches a current model family.
- [ ] The long-context citations (#6, #7, #9) are still the best available — this area moves quickly.
- [ ] No Alammar figure, no 3Blue1Brown frame, and nothing from the excluded deck appears anywhere in the built `.pptx`.
- [ ] Slide 18's U-curve is labelled **"schematic — not measured data."**
