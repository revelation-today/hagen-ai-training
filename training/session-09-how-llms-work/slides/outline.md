# Slides — Session 9: How LLMs Work: From Neural Networks to Claude

Slide-by-slide spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, accessibility, licence-footer rules — not restated here). Target: **16 content slides** + title + agenda + Q&A + resources = **20 slides**, ~45 min. Speaker notes go in the Notes pane, never on the slide. Every headline is a claim, not a label.

---

## Licence quick-reference for this deck — read before building

This session has the **sharpest licence traps in the series**. The best transformer explainers in the world are all unusable on a slide.

| Asset | Verdict | On a slide? |
|---|---|---|
| **Transformer Explainer** (poloclub, MIT) | SLIDE-SAFE | ✅ Live demo **and** screenshots. Attribute "Transformer Explainer, Georgia Tech (MIT)." |
| **Tiktokenizer** (MIT) | SLIDE-SAFE | ✅ Live demo and screenshots. |
| **Hugging Face LLM Course** (Apache-2.0) | SLIDE-SAFE | ✅ Text/code derivable with attribution. |
| **Raschka, LLMs-from-scratch** (Apache-2.0) | SLIDE-SAFE | ✅ Code/figures derivable with attribution. |
| **Karpathy microgpt / nanoGPT** (MIT) | SLIDE-SAFE | ✅ Code. ❌ His videos are link-only. |
| **All Mermaid, tables and Python in this outline** | Original to this course | ✅ Safe to render. |
| **3Blue1Brown** (all rights reserved) | **LINK-ONLY** | ❌ **Never embed, never redraw.** Pre-reading link on the resources slide only. |
| **Jay Alammar, *The Illustrated Transformer*** (**CC BY-NC**) | **LINK-ONLY** | ❌ **Never embed, never redraw.** The NonCommercial clause covers internal corporate training. This is the single most likely mistake in the whole deck — his Q/K/V diagrams are the ones everybody reaches for. |
| **FT visual explainer** | **LINK-ONLY** | ❌ Pre-reading link only. |
| **DeepLearning.AI short course** | **LINK-ONLY** | ❌ Pre-reading link only. |
| **The `Cisco Confidential` deck** | **EXCLUDED** | ❌ Not a source. Nothing on any slide derives from it. See `../resources/sources.md`. |

> **The Snow White slides (9–11) must be built from the Mermaid and numbers in this outline**, which were authored for this course and produced by the code in `../content/03-self-attention.md`. Do not substitute any published rendering of a similar example.

---

## Slide 1 — Title

- **On-slide text:** "How LLMs Work: From Neural Networks to Claude" · Session 9 of 16 · Block: *Do it* · AI Training Series.
- **Speaker notes:** This is the bridge session. Session 6 gave you a neural network; Session 1 gave you "autocomplete on steroids." Today we connect them — you'll see the actual machinery, and by the last slide you'll understand *mechanically* why it hallucinates. Flag the pre-reading: if anyone watched 3Blue1Brown, say so now, they'll enjoy this more.
- **Visual:** Series title layout.
- **Source/licence:** none.

## Slide 2 — Agenda

- **On-slide text:** Tokens · Embeddings · **Attention** · The stack · One token at a time · Context and its costs · Why it hallucinates.
- **Speaker notes:** Mirror the README minute budget. Say out loud that attention is the centre and gets ten minutes; everything before it is setup and everything after is consequence.
- **Visual:** Agenda layout matching `../README.md`.
- **Source/licence:** none.

## Slide 3 — Two questions the same machine has to tell apart (hook)

- **On-slide text:** **"Who is Snow White?"** · **"Why is snow white?"** · Same words. One is a person; one is physics. · *What in the machine could possibly know the difference?*
- **Speaker notes:** Read both aloud. Let the room feel that these are the same words. Then pose the question and explicitly say you are not answering it yet — we're going to build the machine first, and in twenty minutes we'll compute the difference with real numbers. Do not resolve this now; the whole session hangs off the unresolved tension.
- **Visual:** Full-bleed, two lines of large type, nothing else. No diagram.
- **Source/licence:** original.

## Slide 4 — The whole session in one diagram

- **On-slide text:** Text → tokens → embeddings → **transformer blocks** → logits → softmax → sample → *repeat*.
- **Speaker notes:** This is the map for the next 40 minutes. Two things to notice now: the blue box is where meaning happens — everything else is bookkeeping. And the arrow that loops back means the model produces exactly *one* token per pass. That one fact explains streaming, pricing, and latency. Promise to return to the loop.
- **Visual:** Mermaid (original, from `../content/00-overview.md` Figure 1):
  ```mermaid
  flowchart LR
      A["Text"] --> B["Tokens"] --> C["Embeddings<br/>+ position"] --> D["Transformer blocks × N<br/><b>attention + feed-forward</b>"] --> E["Logits"] --> F["Softmax ÷ T"] --> G["Sample<br/>one token"]
      G -.->|"append, run again"| B
      style D fill:#d6eaf8,stroke:#2874a6
  ```
- **Source/licence:** original diagram. Alt text: linear pipeline from text to one sampled token, with a loop back from the sampler to tokenisation.

## Slide 5 — The model never sees your words

- **On-slide text:** Subword pieces, not words · Vocabulary built by **frequency** (BPE), not linguistics · `' Snow'` ≠ `' snow'` ≠ `'snow'` · Nothing is ever "unknown."
- **Speaker notes:** Word-level breaks on unbounded vocabulary; character-level makes sequences too long, and attention punishes length quadratically — we'll see why. BPE is the compromise: merge the most frequent adjacent pair, repeat 50,000 times. The merge list *is* the tokeniser. Emphasise that the leading space and the capital letter are inside the token — that will matter in five minutes.
- **Visual:** Mermaid (original, from `../content/01-tokens.md`): word-level / character-level / subword three-way comparison.
- **Source/licence:** original diagram; BPE description after Hugging Face LLM Course (Apache-2.0) — attribute in footer.

## Slide 6 — Your logs cost 3× your prose

- **On-slide text:** English ~250 tokens / 1,000 chars · German ~300–400 · Code ~350–450 · JSON ~400–500 · Hashes and IDs ~600–900 · *Approximate — measure your own.*
- **Speaker notes:** **Live demo: Tiktokenizer.** Paste the two Snow White sentences, then a German compound, then twenty lines of a real build log. Watch the count jump. The operational point for this room: a config diff or a stack trace is the most expensive thing you can put in a prompt — three to four times worse per character than English. That hits the bill (Session 2) and eats the context window (later today). Also flag: numbers split into arbitrary fragments, which is where unreliable arithmetic starts.
- **Visual:** The cost table + a Tiktokenizer screenshot as network fallback.
- **Source/licence:** **Tiktokenizer (MIT)** — screenshot embeddable, attribute. Table is original; mark it "approximate, verify at delivery."

## Slide 7 — A token becomes a direction, and direction means meaning

- **On-slide text:** One learned vector per vocabulary entry · GPT-2 small: 50,257 × 768 ≈ **38.6M numbers** · Relatedness = cosine similarity = a dot product · No dimension has a human-readable name.
- **Speaker notes:** The embedding layer is a lookup table — the least glamorous component in the model, and often a third of a small model's parameters. Training places tokens that behave alike in similar directions, because that's the cheapest way to predict both well. Say the honest part out loud: the famous king−man+woman analogy is weaker and more contested than its fame suggests, and real dimensions are not interpretable. What survives is: direction encodes relatedness, and that's computed with a dot product — the operation attention is built from.
- **Visual:** Mermaid (original, from `../content/02-embeddings.md`): token ID → embedding matrix → vector, with position added.
- **Source/licence:** original.

## Slide 8 — But the vector for "snow" is the same in both sentences

- **On-slide text:** Embeddings are **context-free** — same row, every time · Position is added, but position is identical here · ✗ frequency models · ✗ static embeddings · **Something has to look at the rest of the sentence.**
- **Speaker notes:** This is the turn. Walk back through what we've built and show that none of it can separate the two questions. A frequency model: both are rare sequences. Static embeddings: same lookup row. Position: `snow` is at slot 3 in both. Pre-2017 language models plateaued for exactly this reason — a system whose word representations don't change with context cannot disambiguate, and natural language is ambiguity all the way down. Now the next slide.
- **Visual:** Mermaid (original, from `../content/03-self-attention.md`): three red ❌ branches — frequency model, static embeddings, position.
- **Source/licence:** original.

## Slide 9 — Attention: every token rebuilds itself out of its context

- **On-slide text:** **Query** — what I'm looking for · **Key** — what I am · **Value** — what I'll contribute · Each token = a **weighted average of all values**, weights it computes itself · `softmax(QKᵀ/√d) · V`
- **Speaker notes:** One sentence carries the whole thing: each token rebuilds its own representation as a weighted mixture of every token's contribution, and decides the weights itself. Decode the formula symbol by symbol — QKᵀ is the n×n "who looks at whom" grid (remember it; it's the cost slide later), √d is numerical hygiene so softmax doesn't saturate, softmax makes each row a distribution, and multiplying by V does the actual mixing. Say clearly: the match is **soft** — nothing is retrieved, everything is blended.
- **Visual:** Mermaid (original, from `../content/03-self-attention.md`): one attention step for a single token — Q/K/V → scores → softmax → weighted sum → new vector.
- **Source/licence:** original diagram. ⚠️ **Do NOT substitute Alammar's Q/K/V figures (CC BY-NC).**

## Slide 10 — Same input vector in, different vector out (the reveal)

- **On-slide text:** `white` starts as the **identical vector** in both sentences · After one attention layer: · *"Who is snow white"* → WHO **0.324**, WHY 0.021 · *"Why is snow white"* → WHO 0.022, WHY **0.276** · **Attention moved the question word into the token.**
- **Speaker notes:** This is the payoff of the hook, and the moment the session turns from description to demonstration. The input vector for `white` was byte-for-byte identical in both runs — only the first token differed, three positions away. The output is different: the token has absorbed the identity of the question word. Say it plainly: *that* is what tells Snow White from snow white. Note also that in the physics question, `white` puts more weight on `snow` (0.325 vs 0.304) — it's binding predicate to subject. Mention that this is a rigged 4-D toy for legibility, but the arithmetic is exactly the real formula.
- **Visual:** The before/after table (from `../content/03-self-attention.md`), with the flipped WHO/WHY columns highlighted. Optionally show the ~20-line numpy block; do not read it aloud.
- **Source/licence:** original — numbers produced by this course's own code.

## Slide 11 — And the difference compounds with depth

- **On-slide text:** Layer 1: weight on `snow` = 0.304 vs **0.325** · Layer 2: 0.356 vs **0.392** · Each layer's queries come from the previous layer's outputs · Depth is not redundancy — **meaning is built up over layers.**
- **Speaker notes:** Why does a model need 12 or 96 layers? Because layer 2's queries are computed from layer 1's already-disambiguated outputs, so the two trajectories diverge further at every step. Early layers do local, syntactic work; later layers assemble larger structures out of what earlier ones resolved. **Then land the caveat, and don't skip it:** attention weights show where information was *routed*, not why or what was concluded. Attention maps are a debugging signal, not an explanation. Same skeptical discipline as the rest of the course.
- **Visual:** Mermaid (original, from `../content/03-self-attention.md`): one input vector, two diverging trajectories through two layers to "the surname" vs "the colour."
- **Source/licence:** original.

## Slide 12 — Live: attention in a real model

- **On-slide text:** *(Demo slide — minimal text)* Transformer Explainer · GPT-2 small, running in your browser · Type a sentence, click a token, watch where it looks.
- **Speaker notes:** **Live demo.** Type both Snow White sentences into the Transformer Explainer and click through the attention maps. This is the moment the toy becomes real for the room. Keep it to 90 seconds — the temperature slider comes back later, so don't spend it now. Have screenshots ready: the demo is a live web tool and the deck must survive with no network.
- **Visual:** Transformer Explainer screenshot (fallback) + the live URL.
- **Source/licence:** **Transformer Explainer, Georgia Tech / Polo Chau lab (MIT)** — screenshots embeddable, attribute in footer.

## Slide 13 — The stack: 12 heads, 12 layers, and where the parameters go

- **On-slide text:** Multi-head: `d_model` **split**, not duplicated (768 = 12 × 64) · Block = attention (*between* tokens) + feed-forward (*within* a token) · Residuals + layer norm make depth trainable · GPT-2 small 124M: FFN **~45%**, embeddings ~31%, attention ~23%.
- **Speaker notes:** Attention moves information between tokens; the feed-forward network transforms it within a token — that's the division of labour. Residual connections (`x + f(x)`) are what let you stack 96 of these without gradients vanishing. The parameter breakdown surprises people: attention gets the fame, the feed-forward layers get the parameters. Use GPT-2 small deliberately — its config is public and verifiable, and the *shape* transfers to models a thousand times larger.
- **Visual:** Mermaid transformer-block diagram (original, from `../content/04-the-transformer-stack.md`) + the parameter table.
- **Source/licence:** original; GPT-2 configuration is public (matches the Transformer Explainer demo model, MIT). ⚠️ **Do not use any frontier-model parameter breakdown from the excluded deck.**

## Slide 14 — Encoder or decoder decides what a model is *for*

- **On-slide text:** **Encoder-only** — bidirectional → representations, classification, **embeddings for RAG** · **Decoder-only** — causally masked → text, left to right → **every chat model you use** · **Encoder-decoder** — sequence to sequence → translation, summarisation.
- **Speaker notes:** The causal mask isn't an optimisation — it's what makes next-token training work at scale, because every position in every training document becomes a training example. Two takeaways for this room: everything you chat with is decoder-only, and the embedding models behind RAG (Session 13) are usually encoder-only, which is why you shouldn't expect them to be interchangeable. If short on time, compress this slide to those two sentences.
- **Visual:** The three-column comparison table from `../content/04-the-transformer-stack.md`.
- **Source/licence:** original table; framing after Hugging Face LLM Course (Apache-2.0) — attribute.

## Slide 15 — It writes one token, then starts over

- **On-slide text:** One full forward pass **per output token** · Streams because it genuinely is produced one at a time · Output tokens cost more than input tokens · **No backspace** — token 1 is committed before token 40 exists.
- **Speaker notes:** Walk the sequence diagram once. Then land the consequence that surprises people most: there is no planning stage. The model does not decide what the answer is and then render it. A confident wrong opening sentence drags the whole answer along, because every later token is conditioned to stay consistent with it. This is also exactly why chain-of-thought prompting works (Session 10) — it turns "think then answer" into "emit intermediate tokens that later tokens can attend to."
- **Visual:** Mermaid `sequenceDiagram` (original, from `../content/05-generating-one-token-at-a-time.md`) — user / runtime / model / sampler, with the loop.
- **Source/licence:** original.

## Slide 16 — Temperature is one division. That's the whole knob.

- **On-slide text:** `P(i) = exp(logit_i / T) / Σ exp(logit_j / T)` · **T=0.5** → Canberra 0.993 · **T=1.0** → 0.907 · **T=2.0** → 0.680, and a **wrong capital ~13% of the time** · **T=0 is reproducible, not accurate.**
- **Speaker notes:** Same model, same prompt, same logits — only the division changes. At T=2 the model answers "Melbourne" or "Perth" to a question it knows perfectly well, 13% of the time. Then the sentence that matters most on this slide: **turning temperature down reduces variance, not error.** The most probable fabrication is still a fabrication — it's just a reproducible one. That misconception is common and expensive in engineering teams. Mention top-p briefly (adaptive tail truncation, prefer it to top-k, vary one knob at a time) and note that true reproducibility also needs a pinned model version and a logged full prompt.
- **Visual:** The three-row temperature table, plus the Transformer Explainer temperature slider **live** if network allows (it recomputes the distribution in front of the room).
- **Source/licence:** numbers original (this course's own code). Demo: **Transformer Explainer (MIT)** — attribute.

## Slide 17 — Doubling the context quadruples the attention cost

- **On-slide text:** Every query × every key = an **n × n grid**, per head, per layer · 1K → 1M comparisons · 16K → 268M · 64K → **4.3 billion** · 1M → **1.1 trillion** · KV cache: memory grows **linearly**, tens of GB per conversation.
- **Speaker notes:** Point back to slide 9 — QKᵀ is that grid, and this is the bill for it. A 96-layer, 96-head model computes over 9,000 of them per pass. The KV cache is what stops generation being O(n³): keys and values never change once computed, so cache them — at the price of memory that grows with length, which is the real limit on how many users a GPU serves at once. Two practical hooks: prompt caching (Session 2's discount) is this cache persisted, which is why it needs a **stable prefix** — put volatile content last. And time-to-first-token is prefill; inter-token latency is the cheap part.
- **Visual:** The n² cost table + the original Mermaid cost-chain diagram from `../content/06-the-context-window.md`.
- **Source/licence:** original.

## Slide 18 — More context is not monotonically better

- **On-slide text:** **Lost in the middle** — accuracy is **U-shaped** in the position of the relevant fact · **Context rot** — degrades with length **even at 100% retrieval** · Distractors bite harder as length grows · **Curate, don't dump.** Critical material first or last.
- **Speaker notes:** This is the most transferable finding in the session for this room. Everyone's intuition is that context is like RAM — if it fits, it's available. It isn't. Liu et al. found the U-shape; Chroma's replication across 18 models found degradation with length even when retrieval is demonstrably perfect. Mechanically it's not mysterious: softmax distributes a fixed budget of attention mass across more competitors, position handling is least practised in the interior, and more text means more plausible near-misses. So: *"I gave it all the logs and it got worse"* is not user error and not a bug — it's documented behaviour. Measure it on your own workload.
- **Visual:** The schematic U-curve (`xychart-beta` from `../content/06-the-context-window.md`) — **clearly labelled "schematic, not measured"** on the slide itself.
- **Source/licence:** ⚠️ Cite Liu et al. (Lost in the Middle) and Chroma (Context Rot) as **claims**; **do not reproduce their figures**. Our curve is an original schematic and must be labelled as such.

## Slide 19 — Now walk the pipeline and find the fact-check

- **On-slide text:** Tokenise? No. Embed? No. Attention? Computes **relevance**, not correctness. Feed-forward? Learned weights. Logits? "Fits the pattern." Softmax? Arithmetic. Sample? A die roll. · **There is nowhere for a truth check to be.**
- **Speaker notes:** This is the promise Session 1 made, and it's the last three minutes. Go stage by stage, out loud, and ask the question each time. The point isn't that the check is weak — there is no component whose job is verification and no signal available to one. The only training objective was next-token probability. **Correct answers and fabrications are produced by identical arithmetic.** Then the confidence point: a fabricated citation can have a *sharper* distribution than a true fact, because the format is a very strong pattern. Fluency is the training objective, not a reliability signal. Close by naming the misconception one more time: temperature 0 does not fix this.
- **Visual:** Mermaid (original, from `../content/07-why-it-hallucinates.md`): the pipeline with "where would a truth check live?" pointing at three stages and finding nowhere.
- **Source/licence:** original. Session 1 framing ("autocomplete on steroids") is paraphrased from a LINK-ONLY source — attribute the framing verbally, embed nothing.

## Slide 20 — Q&A / discussion

- **On-slide text:** *"Given the mechanism, which of our AI use cases is the machine structurally unsuited for?"* · See `exercises/discussion.md`.
- **Speaker notes:** Open with that seed. The good answers name a *stage* — "we're asking it for exact part numbers, and numbers get shredded at tokenisation," or "we dump the whole incident history in, and that's context rot." Have the two backup prompts ready: the temperature-0 misconception and the "we'll just use a bigger window" procurement question.
- **Visual:** Discussion/poll layout.
- **Source/licence:** none.

## Slide 21 — Resources & credits

- **On-slide text:** **Play with it:** Transformer Explainer (MIT) · Tiktokenizer (MIT) · Karpathy microgpt (MIT) · **Read/watch:** 3Blue1Brown Ch. 5–6 · Alammar, *The Illustrated Transformer* · FT explainer · Hugging Face LLM Course · Raschka, *LLMs-from-scratch*.
- **Speaker notes:** Point out that the three "play with it" links are the ones to open tonight — all client-side, no login. Say explicitly that 3Blue1Brown and the Illustrated Transformer are the best explanations in existence and that's exactly why they're links rather than slides: their licences don't permit reuse in corporate training, and we respect that.
- **Visual:** Links + licence attributions from `../resources/sources.md`.
- **Source/licence:** attribution slide — MIT/Apache credits for every embedded asset used in this deck.

---

## Build checklist specific to this deck

- [ ] **No Alammar figure, no 3Blue1Brown frame, nothing from the Cisco deck appears anywhere.** Check every slide.
- [ ] Slides 9–11 use only the Mermaid and numbers from this outline.
- [ ] Slide 18's curve is labelled **"schematic — not measured data."**
- [ ] Screenshots captured for Tiktokenizer and Transformer Explainer (slides 6, 12, 16) so the deck runs with no network.
- [ ] MIT/Apache attribution footers on slides 6, 12, 13, 14, 16.
- [ ] Slide 3's hook is **not** resolved before slide 10.
- [ ] Rehearsed at 45 min with the attention block (9–12) at a full 10 minutes.
