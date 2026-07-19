# Session 9 — How LLMs Work: From Neural Networks to Claude

**Block:** Do it · **Goal covered:** 3 ("LLM") · **Format:** 45 min content + 15 min Q&A · **Hands-on:** optional 25-min lab (`exercises/lab.md`)

---

## One-paragraph summary

Session 6 showed you a neural network: weighted sums, layers, a number coming out the other end. Session 1 told you an LLM is "autocomplete on steroids." This session is the bridge between those two statements — it shows you the actual machinery that turns a pile of matrix multiplications into the thing you type into every day. We follow one sentence all the way through: **text → tokens → embeddings → attention → a probability distribution over the next token → one sampled token → repeat.** The centrepiece is **self-attention**, taught through a single minimal pair — *"Who is Snow White?"* versus *"Why is snow white?"*. Nearly the same words; completely different meanings; **attention is the mechanism that tells them apart**, and we compute that difference with real numbers you can run. From there the practical consequences fall out for free: why generation is one token at a time, what the temperature knob actually does to the arithmetic, why the context window is finite and costs **quadratically**, and why stuffing more context in eventually makes answers *worse*. The session closes by paying off Session 1: now that you have seen every step, you can see that **not one of them checks the output against truth**. Hallucination is not a bug bolted onto this machine — it is what this machine does when the pattern is thin.

## Audience & level

Qualcomm release / problem / configuration managers and developers, carrying the neural-network grounding from Session 6 and the token/cost vocabulary from Session 2. This is a **concept session with runnable code**, not a lab session — the Python is there to make the mechanism concrete and can be read rather than executed. No calculus. One dot product, explained. Managers get the parts that govern procurement and risk decisions (context cost, temperature, why "just give it more documents" degrades); developers get the mechanism in enough depth to reason about latency, cost, and failure.

## Learning objectives

After this session a participant can:

1. **Trace** a prompt through the full LLM pipeline — tokenisation, embedding, attention, logits, sampling — and say what each stage does and does not do.
2. **Explain** why a token is not a word, and predict which inputs (code, German compounds, rare names, numbers) tokenise expensively.
3. **Explain self-attention** using the *Snow White* minimal pair: identical token vectors going in, different vectors coming out, because each token's representation is rebuilt as a weighted mixture of its context.
4. **Describe** why generation is autoregressive (one token at a time) and what **temperature**, top-k and top-p do to the probability distribution — including when to set temperature to 0.
5. **Justify** why context costs **O(n²)** in attention and linear memory in the KV cache, and quantify what doubling context does to the bill and the latency.
6. **Explain long-context degradation** ("context rot", lost-in-the-middle) and state the engineering rule that follows: more context is not free and not monotonically better.
7. **Explain, mechanically, why an LLM hallucinates** — naming the exact place in the pipeline where a truth check would have to go, and observing that nothing is there.

## Prerequisites

- **Session 1** — "autocomplete on steroids; pattern-matcher, not search engine," and the intrinsic/extrinsic hallucination split. This session is the *mechanical* explanation of the claim Session 1 made *behaviourally*. The payoff in `content/07` depends on it.
- **Session 2** — token, context window, input vs. output tokens, the cost meter. We do not re-teach the pricing; we explain *why* the meter runs the way it does.
- **Session 6** — a neuron is a weighted sum plus a nonlinearity; layers stack; training adjusts weights to reduce error. A transformer is a particular arrangement of exactly those parts.
- Optional but useful: Sessions 3–5 for the "cost/distance we minimise" spine.

### Pre-reading (assign in advance — link only, never embedded)

These are the best explanations in existence and they are all **LINK-ONLY** for licence reasons (see `resources/sources.md`). Assign them; do not put their figures in the deck.

| Assign | Why | Time |
|---|---|---|
| 3Blue1Brown, *Transformers* (Ch. 5) and *Attention in transformers* (Ch. 6) | Best geometric intuition for what attention does in embedding space | ~50 min |
| Jay Alammar, *The Illustrated Transformer* | Clearest static walkthrough of Q/K/V ever made (**CC BY-NC** — pre-read only) | ~35 min |
| FT, *Generative AI exists because of the transformer* | Best narrative framing for non-specialists | ~15 min |

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–3 min | **Hook — the minimal pair** | Put *"Who is Snow White?"* and *"Why is snow white?"* on one slide. Ask the room: same words. What in the machine could possibly tell these apart? Leave it hanging. |
| 3–9 min | **Tokens** | Text is chopped into subword pieces before anything else happens. Live: Tiktokenizer. Why `" Snow"` and `" snow"` are different tokens, and why that matters. |
| 9–15 min | **Embeddings** | Each token becomes a vector. Direction ≈ meaning. Similarity is a dot product. But the vector for a token is *context-free* — which is exactly the problem the hook posed. |
| 15–25 min | **Self-attention — the payoff** | Q/K/V at intuition level; each token rebuilds itself as a weighted mixture of the others. **Resolve the hook with real numbers.** Live: Transformer Explainer attention map. |
| 25–30 min | **The stack** | Multi-head; 12 (or 96) layers of the same block; encoder vs. decoder; what "124M parameters" is made of. |
| 30–37 min | **One token at a time** | Logits → softmax → sample → append → repeat. **Temperature**, top-k, top-p, with the arithmetic shown. Live: Transformer Explainer temperature slider. |
| 37–42 min | **The context window** | Why attention is O(n²); the KV cache; the cost table; **context rot** and lost-in-the-middle. |
| 42–45 min | **Why it hallucinates** | Walk the pipeline backwards and ask "where is the fact check?" There isn't one. Session 1, now mechanically explained. |
| 45–60 min | **Q&A / discussion** | See `exercises/discussion.md`. Seed: *"Given the mechanism, which of our team's AI use cases is the machine structurally unsuited for?"* |

**Is 45 minutes honest?** It is tight but achievable, on one condition: **the attention segment is protected**. It is the session. If you run long, cut in this order — (1) the encoder-vs-decoder table in `content/04` to a single sentence, (2) top-k/top-p detail (keep temperature), (3) the parameter-budget breakdown. Do **not** cut the Snow White resolution or the hallucination payoff; the first is the teaching centre and the second is the promise Session 1 made.

## Materials & tools

- Slides: `slides/outline.md` → deck built per `../powerpoint_instructions.md`.
- Self-study reading: `content/00-overview.md` → `content/99-key-takeaways.md`.
- **Live demo 1 — Tiktokenizer** (`tiktokenizer.vercel.app`, MIT). Paste text, see token boundaries. Client-side, no login.
- **Live demo 2 — Transformer Explainer** (`poloclub.github.io/transformer-explainer`, MIT). GPT-2 small running in the browser: attention maps, next-token probabilities, temperature slider. **This is the session's workhorse demo — screenshots are licence-safe as a fallback.**
- **Optional demo 3 — Karpathy's microgpt** (MIT): a complete GPT in 200 lines of dependency-free Python, readable top to bottom on one screen.
- Lab (optional, ~25 min, Colab-first): `exercises/lab.md` — tokenise, embed, hand-compute attention, and watch temperature change the output.
- Self-check: `exercises/quiz.md`. Discussion prompts: `exercises/discussion.md`.

> **Network fallback.** All three demos are live web tools. Take screenshots the day before. The deck must be presentable with no network.

## Source & licence note

This session is **largely authored**. The corpus's only deep transformer treatment sits in the `Cisco Confidential` deck, which is **excluded** from this course (`output/AI_input.md` §1). The DL course gives transformers a single bullet. Everything here is therefore rebuilt from licence-checked public material, and every diagram, table, and code block is original to this course.

| Source | Role | Verdict |
|---|---|---|
| **Transformer Explainer** (Georgia Tech / Polo Chau lab) | The live demo; screenshots on slides | **SLIDE-SAFE** (MIT — attribute) |
| **Hugging Face LLM Course** | Tokenisation depth; encoder/decoder framing | **SLIDE-SAFE** (Apache-2.0) |
| **Raschka, *LLMs-from-scratch*** | Reference implementation for the attention arithmetic | **SLIDE-SAFE** (Apache-2.0) |
| **Karpathy microgpt / nanoGPT** | Optional "see it run" demo | **SLIDE-SAFE** (MIT) — videos link-only |
| **Tiktokenizer** | Tokenisation live demo | **SLIDE-SAFE** (MIT) |
| **Chroma, "Context Rot"** · **Liu et al., "Lost in the Middle"** | The long-context degradation evidence | Cite as claims; **do not reuse figures** |
| **3Blue1Brown · Alammar's *Illustrated Transformer* · FT explainer · DeepLearning.AI short course** | Pre-reading | **LINK-ONLY** — never embed |
| **`Cisco Confidential` deck** | — | **EXCLUDED.** Not a source. Not cited. |

> **On the *Snow White* example.** The minimal pair is a natural English ambiguity, and a version of it appears in the excluded deck. We are not reusing that deck's material: the framing, the toy vectors, the computed attention weights, the diagrams, and the code in `content/03` are **written from scratch for this course**. Nothing is copied, and the excluded deck is not listed as a source. Full reasoning in `resources/sources.md`.

Full provenance, per-item licence verdicts, and the "verify before delivery" list: `resources/sources.md`.
