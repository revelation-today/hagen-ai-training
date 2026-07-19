# Lab — Watch the Mechanism Work (~25–30 min)

An optional hands-on lab for Session 9. It is not required to follow the session — the concept content stands alone — but it is the fastest way to make attention stop feeling like a metaphor.

**Format:** four short steps. You will tokenise real text, watch context change a token's meaning, control a sampler with temperature, and measure the quadratic cost of context. Only Step 1 needs an install; Steps 2–4 need nothing but `numpy`.

---

## Setup

**Colab (recommended).** Open a new notebook at `colab.research.google.com`. `numpy` is pre-installed; install `tiktoken` in the first cell.

```python
!pip -q install tiktoken
```

**JupyterLite fallback** (no account, runs in the browser: `jupyter.org/try-jupyter/lab/`). `tiktoken` may not install there — if so, **skip Step 1's code and use the Tiktokenizer web tool** (`tiktokenizer.vercel.app`) instead. Steps 2–4 work fine.

**No-install fallback for everything:** Steps 2–4 are pure `numpy` and will run in any Python 3 environment you already have.

---

## Step 1 — Tokenise your own text (5 min)

```python
import tiktoken
enc = tiktoken.get_encoding("o200k_base")

def show(text):
    ids = enc.encode(text)
    pieces = [enc.decode([i]) for i in ids]
    print(f"{len(ids):4d} tokens | {pieces}")

show("Who is Snow White?")
show("Why is snow white?")
show("who is snow white")           # no capitals — humans still read it as the fairy tale
show("Konfigurationsmanagement")
show("QCT-8895-rev3")

#   5 tokens | ['Who', ' is', ' Snow', ' White', '?']
#   5 tokens | ['Why', ' is', ' snow', ' white', '?']
#   4 tokens | ['who', ' is', ' snow', ' white']
#  ~6 tokens | ['K', 'onfig', 'ur', 'ations', 'management']
#  ~7 tokens | ['Q', 'CT', '-', '88', '95', '-rev', '3']
# Exact splits vary by encoding — the shape of the result is the lesson, not the IDs.
```

**Now do it with your own material.** Paste in 20 lines of a real build log, a config diff, or a JSON manifest — anything non-confidential from your actual work. Compute tokens per character.

```python
sample = """<paste 20 lines of your own log / diff / manifest here>"""
n = len(enc.encode(sample))
print(f"{n} tokens for {len(sample)} chars = {1000*n/len(sample):.0f} tokens per 1,000 chars")
# English prose lands near 250. Note how far above that your real artefact sits.
```

**What to notice:** how far your real working artefacts sit above English prose, and how numbers and identifiers fragment.

---

## Step 2 — Make context change a word's meaning (10 min) — *the important one*

This is the `content/03` demonstration. Run it as-is first.

```python
import numpy as np
np.set_printoptions(precision=3, suppress=True)

# Toy 4-D space with named axes: [WHO, WHY, SNOW, WHITE].
# 'snow' and 'white' are the SAME vectors in both sentences.
E = {"Who":   np.array([1., 0., 0., 0.]),
     "Why":   np.array([0., 1., 0., 0.]),
     "is":    np.array([.1, .1, .1, .1]),
     "snow":  np.array([0., 0., 1., 0.]),
     "white": np.array([0., 0., 0., 1.])}

W_q = np.array([[0, 0, 0, 0],
                [0, 0, 0, 0],
                [0, 0, 1, 0],
                [1.0, 0.5, 1.0, 0]], float)   # learned in a real model; hand-set here
W_k = np.eye(4)
W_v = np.eye(4)

def layer(X):
    Q, K, V = X @ W_q, X @ W_k, X @ W_v
    S = Q @ K.T / np.sqrt(4)
    A = np.exp(S - S.max(1, keepdims=True)); A /= A.sum(1, keepdims=True)
    return A, X + A @ V                        # residual connection

for sent in (["Who", "is", "snow", "white"], ["Why", "is", "snow", "white"]):
    X = np.stack([E[t] for t in sent])
    A1, H1 = layer(X)
    A2, H2 = layer(H1)
    print(" ".join(sent))
    print("  layer1 attention row for 'white':", A1[3])
    print("  layer1 output       for 'white':", H1[3])
    print("  layer2 attention row for 'white':", A2[3])
    print("  layer2 output       for 'white':", H2[3])

# Who is snow white
#   layer1 attention row for 'white': [0.304 0.209 0.304 0.184]
#   layer1 output       for 'white': [0.324 0.021 0.324 1.205]
#   layer2 attention row for 'white': [0.288 0.188 0.356 0.168]
#   layer2 output       for 'white': [0.9   0.063 1.02  1.639]
# Why is snow white
#   layer1 attention row for 'white': [0.253 0.224 0.325 0.197]
#   layer1 output       for 'white': [0.022 0.276 0.348 1.22 ]
#   layer2 attention row for 'white': [0.228 0.202 0.392 0.178]
#   layer2 output       for 'white': [0.066 0.783 1.09  1.665]
```

**What to verify for yourself:**
1. The input vector for `white` is identical in both runs. Print `E["white"]` if you want to be sure.
2. The layer-1 output differs in exactly the WHO/WHY channels: 0.324/0.021 versus 0.022/0.276.
3. The weight `white` puts on `snow` is higher in the physics question (0.325 vs 0.304) and the gap **widens** at layer 2 (0.392 vs 0.356).

**Break it — three challenges:**
- **Delete the residual connection** (`return A, A @ V`). What happens to the WHITE channel of the output, and why does that make deep stacks hard to train?
- **Remove the `/ np.sqrt(4)` scaling** and multiply all embeddings by 10. Watch the attention row collapse toward one-hot. This is the softmax saturation the `√d` term exists to prevent.
- **Add a third sentence** — `["Who", "is", "snow", "white"]` with `Who`'s vector set to `[0.5, 0.5, 0, 0]` (an ambiguous question word). Where does the output for `white` land relative to the two originals?

---

## Step 3 — Drive the sampler with temperature (5 min)

```python
import numpy as np

logits = np.array([8.0, 5.5, 4.0, 2.0])
names = ["Canberra", "Sydney", "Melbourne", "Perth"]

def probs(logits, T):
    z = logits / T
    p = np.exp(z - z.max()); return p / p.sum()

for T in (0.5, 1.0, 2.0):
    print(f"T={T}: " + "  ".join(f"{n}={v:.3f}" for n, v in zip(names, probs(logits, T))))
# T=0.5: Canberra=0.993  Sydney=0.007  Melbourne=0.000  Perth=0.000
# T=1.0: Canberra=0.907  Sydney=0.074  Melbourne=0.017  Perth=0.002
# T=2.0: Canberra=0.680  Sydney=0.195  Melbourne=0.092  Perth=0.034

# Now actually sample 1,000 times at each temperature and count the wrong answers.
rng = np.random.default_rng(0)
for T in (0.5, 1.0, 2.0):
    draws = rng.choice(names, size=1000, p=probs(logits, T))
    wrong = (draws != "Canberra").sum()
    print(f"T={T}: wrong answer in {wrong/10:.1f}% of 1,000 draws")
# T=0.5: wrong answer in 0.7% of 1,000 draws
# T=1.0: wrong answer in 9.5% of 1,000 draws
# T=2.0: wrong answer in 30.9% of 1,000 draws
# (with seed 0; the counts move a little with a different seed)
```

**Extend it:** implement top-p. Sort the probabilities descending, keep the smallest prefix summing to `p`, renormalise, and sample from that. Then check what `p=0.9` keeps at `T=1.0` (only *Canberra*) versus at `T=2.0` (three candidates). That adaptiveness is the entire argument for top-p over top-k.

---

## Step 4 — Feel the quadratic (5 min)

```python
import numpy as np, time

d = 64
for n in (256, 512, 1024, 2048, 4096):
    Q = np.random.randn(n, d); K = np.random.randn(n, d)
    t0 = time.perf_counter()
    S = Q @ K.T                      # the n x n score grid — ONE head, ONE layer
    t = time.perf_counter() - t0
    print(f"n={n:5d}  grid={n*n:>12,} entries  {S.nbytes/1e6:7.1f} MB  {t*1000:7.1f} ms")

# n=  256  grid=      65,536 entries      0.5 MB    ~0.4 ms
# n= 512  grid=     262,144 entries      2.1 MB    ~1.4 ms
# n= 1024  grid=   1,048,576 entries      8.4 MB    ~5 ms
# n= 2048  grid=   4,194,304 entries     33.6 MB   ~20 ms
# n= 4096  grid=  16,777,216 entries    134.2 MB   ~80 ms
# Timings depend entirely on your machine. The RATIOS are the lesson:
# each doubling of n multiplies grid size and time by roughly 4.
```

**Then reason about it:** multiply the 4096-row figure by 12 heads × 12 layers (GPT-2 small). Then imagine 96 × 96. Then imagine n = 128,000 rather than 4,096. That is why long context is an architecture research problem and not a config flag.

---

## If you have another 10 minutes

Open **Karpathy's microgpt** (MIT) — a complete GPT in about 200 lines of dependency-free Python: dataset, tokenizer, autograd, the transformer, Adam, training, and inference. Read it top to bottom. Everything in this session is in there, and you can find each stage by name. It is the best "so that's *all* it is" artefact currently available. Link in `../resources/sources.md`.

---

## Success criteria

You have got what this lab is for if you can now say, without looking anything up:

- Why your build log costs three times more per character than an English paragraph.
- Which numbers in Step 2 prove that context — and only context — changed the meaning of `white`.
- What temperature multiplies, divides, or otherwise does to the arithmetic, and why `T=0` does not make a model accurate.
- What quantity in a transformer grows with n², and what that means for a "just use a bigger window" proposal.
