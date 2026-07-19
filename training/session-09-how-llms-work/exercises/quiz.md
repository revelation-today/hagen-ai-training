# Self-Check Quiz — Session 9

Ten questions. Answer them before looking at the key. If you get 8+, you have the mechanism.

---

**1.** Why do LLMs use subword tokenisation rather than one token per word, or one token per character?

**2.** You paste a 4,000-character JSON build manifest into a prompt and a 4,000-character English summary of the same build. Which costs more tokens, roughly how much more, and why?

**3.** Sentence A is `"Who is Snow White?"` and sentence B is `"Why is snow white?"`. The token `snow`/`Snow` and `white`/`White` fetch vectors from a fixed embedding table. Explain in one sentence why that table cannot possibly distinguish the two sentences.

**4.** In `softmax(QKᵀ/√d)V`, say what each of the four elements contributes — `Q Kᵀ`, `√d`, `softmax`, and the final `V`.

**5.** In the worked example, the *input* vector for `white` was identical in both sentences but the *output* vector differed. What single fact about the two sentences caused that difference, and by what mechanism did it reach the token `white`?

**6.** Your colleague looks at an attention heat map and says "see, it attended to the error code — that's why it flagged the build." What is wrong with that inference?

**7.** A model is set to `temperature = 0`. Which of these does that guarantee: (a) accuracy, (b) that the sampler is deterministic, (c) that identical prompts always return identical text? Explain any you rule out.

**8.** Your context grows from 8,000 tokens to 32,000 tokens. What happens to (a) the attention score-grid size, (b) the KV-cache memory?

**9.** A team reports: "we started passing the model the full incident history instead of just the last three tickets, and the summaries got *worse*." Is this a bug in the model, a bug in their prompt, or expected behaviour? Name the effect.

**10.** Walk the pipeline — tokenise, embed, attention, feed-forward, logits, softmax, sample — and identify the stage that checks whether the output is true.

---
---

## Answer key

**1.** Word-level fails because the vocabulary is unbounded — new names, identifiers, and typos appear forever, so you need an `<unk>` token that destroys information — and because morphological relatives (`configure`/`configured`/`configuring`) become unrelated integers. Character-level fixes both but makes sequences four to five times longer, and attention cost grows with the **square** of sequence length, so that is very expensive. Subword (BPE) is the compromise: frequent words are single tokens, rare words are assembled from frequent fragments, and nothing is ever unknown because the worst case falls back to bytes.

**2.** The JSON costs substantially more — roughly 400–500 tokens per 1,000 characters versus ~250 for English prose, so about **1.6–2× more**. Structural punctuation (`{`, `"`, `:`, `,`) approaches one token per character, and identifiers and hashes fragment because BPE's merge list was built on a corpus dominated by prose.

**3.** Because an embedding table is a **context-free lookup**: token ID → the same row, every time, regardless of what surrounds it. Nothing in that operation consults the rest of the sentence, so the vector for `snow` is identical in a fairy-tale question and a physics question.

**4.** `Q Kᵀ` computes every query against every key, producing the *n × n* grid of relevance scores — and this is where the quadratic cost lives. `√d` (the key dimension) scales the scores down; without it, dot products of high-dimensional vectors grow large and softmax saturates into a near-one-hot spike, which kills gradients during training. `softmax` turns each row into positive weights summing to 1, making it a genuine weighted average. Multiplying by `V` performs the actual mixing: each output row is that token's chosen blend of context.

**5.** The single difference is the first token — `Who` versus `Why`, three positions earlier. It reached `white` through **self-attention**: `white`'s query was dotted against every key including the question word's, the resulting weight let the question word's **value** vector into `white`'s weighted average, and the output therefore carries "identity question" versus "causal question." (Bonus: the divergence grows at layer 2 because layer 2's queries are computed from layer 1's already-differing outputs.)

**6.** Attention weights show where information was **routed**, not why or what was concluded. A model can attend to a token and do nothing useful with it; information also flows through residual connections and feed-forward layers that no attention map shows; and a many-layer, many-head model has thousands of such maps that do not compose into a narrative. A heat map is a debugging signal, not an explanation — and treating it as explainability is a real procurement risk.

**7.** Only **(b)**. Temperature 0 makes the *sampler* deterministic (always take the highest logit). It does **not** give accuracy — you get the most probable output, and the most probable completion of a thin pattern is still a fabrication, just a reproducible one. It also does not by itself give (c): identical prompts can still differ because of floating-point non-determinism in batched GPU kernels, silent model-version updates behind a stable API name, and system-prompt text you did not author. Reproducibility needs a pinned version and a logged, byte-identical prompt as well.

**8.** (a) The score grid goes from 8,000² = 64 million entries to 32,000² = **1.024 billion** — a **16×** increase for a 4× increase in length, per head per layer. (b) The KV cache grows **linearly**, so 4× — large in absolute terms but not quadratic. This asymmetry is why the KV cache is the fix for repeated recomputation and simultaneously the limit on how many concurrent conversations a GPU can hold.

**9.** **Expected behaviour**, and it has two names: **lost in the middle** (accuracy is U-shaped in the position of the relevant fact — material in the interior of a long context is used least well) and **context rot** (performance degrades as input length grows even when retrieval is demonstrably perfect, and distractors bite harder at length). The fix is not a better prompt but a better pipeline: retrieve and rank, curate rather than dump, and put critical material at the start or the end.

**10.** **There isn't one.** Tokenising is string splitting; embedding is a table lookup; attention computes relevance, which is not correctness; the feed-forward layers apply learned weights that encode statistical association, not verified propositions; a logit says "fits the pattern," not "is true"; softmax is arithmetic; sampling is a weighted die roll. The only training objective was next-token probability. Correct answers and confident fabrications are produced by **identical arithmetic**, which is why verification must be added from outside the model.

---

### Scoring

| Score | Reading |
|---|---|
| 9–10 | You can explain the mechanism to someone else. Go argue with the Session 14 material. |
| 6–8 | Solid. Re-read `content/03` (attention) and `content/06` (context) for the gaps. |
| 3–5 | Re-read `content/00` and `content/03`, then run the Step 2 lab — the numbers do more than the prose. |
| 0–2 | Start again at `content/00-overview.md` and follow the pipeline diagram file by file. |
