# Lab — Session 2: Count the Tokens, Then Price Them

**Time:** ~25–30 minutes · **Difficulty:** low (no ML, no API key, no cost) · **Prerequisite:** a browser.

You will measure real token counts on real text, then build a cost estimator and use it to reproduce — and then break — the numbers from `content/04` and `content/05`. Nothing here calls a model, so **the lab costs nothing and needs no credentials.**

Non-coders: Part 1 is browser-only and carries most of the lesson. Pair with a colleague for Parts 2–5 and read the output together — the point is the *numbers*, not the Python.

---

## Setup

**Primary — Google Colab (recommended).**
1. Open `https://colab.research.google.com` → *New notebook*.
2. First cell: `!pip install tiktoken` → run. Takes ~15 seconds.

**Fallback — JupyterLite** (`https://jupyter.org/try-jupyter/lab/`), fully in-browser, no account. `tiktoken` may not install under Pyodide; if it doesn't, use the **estimator fallback** given in Part 2 (it uses a characters-per-token approximation and still lets you do Parts 3–5).

**Second fallback — anything with Python 3.9+**: `pip install tiktoken` locally.

---

## Part 1 — See the split (browser only, 5 min)

Open `https://platform.openai.com/tokenizer`. Free, no login. Paste each of these and **write down the token count**:

| # | Paste this | Your count | Expect roughly |
|---|---|---|---|
| 1 | `The release is blocked by a configuration mismatch.` | | ~10 |
| 2 | `Die Konfigurationsverwaltung blockiert die Freigabe.` | | ~15 |
| 3 | `configuration` | | 1 |
| 4 | `Konfigurationsverwaltung` | | ~7 |
| 5 | `{"ticket_id": "QCA-88412", "severity": 1, "reopened": 3}` | | ~25 |
| 6 | `2026-07-19T04:11:57.884Z [ERROR] wlan_host: assert failed at qca6390_init.c:2214` | | ~35 |
| 7 | Your own name | | 1–2 |
| 8 | The most unusual surname on your team | | 3–5 |

Turn on the coloured-segment view so you can see *where* the splits fall. Two things to notice and write down:

- Rows 3 and 4 are both one word. **One is one token; the other is seven.**
- In row 6, count how many tokens the *timestamp alone* consumes. That is a field your logging framework emits on every line and that you may be paying to send.

> **Question to answer before moving on:** for the same *meaning*, which of your team's common inputs is most expensive per unit of information — English prose, German, code, JSON, or log lines? You'll verify your guess in Part 2.

---

## Part 2 — Measure it (7 min)

```python
# Cell 1
!pip install -q tiktoken

# Cell 2
import tiktoken

enc = tiktoken.get_encoding("o200k_base")   # a modern OpenAI vocabulary

def count_tokens(text: str) -> int:
    return len(enc.encode(text))

samples = {
    "english_prose": "The release is blocked by a configuration mismatch in the build pipeline.",
    "german":        "Die Konfigurationsverwaltung blockiert die Freigabe der Auslieferung.",
    "code":          "def get_ticket_status(ticket_id):\n    return db.query(Ticket).get(ticket_id).status",
    "json":          '{"ticket_id": "QCA-88412", "severity": 1, "component": "wlan_host", "reopened": 3}',
    "log_line":      "2026-07-19T04:11:57.884Z [ERROR] wlan_host: assert failed at qca6390_init.c:2214 (rc=-22)",
}

print(f"{'sample':<15}{'words':>7}{'tokens':>8}{'tok/word':>10}{'chars/tok':>11}")
for name, text in samples.items():
    t, w, c = count_tokens(text), len(text.split()), len(text)
    print(f"{name:<15}{w:>7}{t:>8}{t/w:>10.2f}{c/t:>11.2f}")

# Expected output (approximate — exact counts depend on the encoding version):
# sample           words  tokens  tok/word  chars/tok
# english_prose       12      14      1.17       5.21
# german               8      19      2.38       4.05
# code                 8      24      3.00       3.75
# json                10      34      3.40       2.62
# log_line            10      39      3.90       2.51
```

**Read the `chars/tok` column.** English prose gets ~5 characters per token; a log line gets ~2.5. **The tokeniser compresses English prose twice as well as it compresses your logs.** That is the whole "content type matters" lesson in one number.

Now see the actual pieces:

```python
# Cell 3
for word in ["configuration", "Konfigurationsverwaltung", "QCA6390", "wlan_host"]:
    ids = enc.encode(word)
    print(f"{word:<26} {len(ids):>2} tokens  {[enc.decode([i]) for i in ids]}")

# Expected output (approximate):
# configuration               1 tokens  ['configuration']
# Konfigurationsverwaltung    7 tokens  ['Kon', 'fig', 'ur', 'ations', 'ver', 'wal', 'tung']
# QCA6390                     4 tokens  ['Q', 'CA', '63', '90']
# wlan_host                   3 tokens  ['w', 'lan', '_host']
```

> **JupyterLite fallback if `tiktoken` won't install:** replace `count_tokens` with
> `def count_tokens(text): return max(1, round(len(text) / 3.6))`
> — a crude chars-per-token approximation. It is wrong by 10–25 % and it is fine for Parts 3–5. Say out loud that you are estimating, not measuring.

---

## Part 3 — Build the cost estimator (7 min)

```python
# Cell 4
from dataclasses import dataclass

@dataclass(frozen=True)
class Tier:
    name: str
    price_in: float           # USD per 1M input tokens
    price_out: float          # USD per 1M output tokens
    price_cached_in: float    # USD per 1M cached input tokens

# ---- ILLUSTRATIVE prices (as of mid-2026). VERIFY against vendor pricing ----
# ---- pages before using this for anything real.                          ----
TIERS = [
    Tier("A - frontier",  15.00, 75.00, 1.50),
    Tier("B - workhorse",  3.00, 15.00, 0.30),
    Tier("C - small",      0.25,  1.25, 0.025),
]

def cost_per_call(tier, input_tokens, output_tokens, cached_input_tokens=0):
    fresh = max(input_tokens - cached_input_tokens, 0)
    return (fresh                * tier.price_in        / 1e6
            + cached_input_tokens * tier.price_cached_in / 1e6
            + output_tokens       * tier.price_out       / 1e6)

def report(label, tok_in, tok_out, calls, cached=0):
    print(f"\n{label}  ({tok_in:,} in / {tok_out:,} out"
          f"{f' / {cached:,} cached' if cached else ''} x {calls:,} calls)")
    print(f"  {'tier':<14}{'per call':>12}{'per month':>13}{'per year':>13}")
    for t in TIERS:
        c = cost_per_call(t, tok_in, tok_out, cached)
        print(f"  {t.name:<14}{c:>12.6f}{c*calls:>13.2f}{c*calls*12:>13.2f}")

report("X - ticket only",            1_400, 300, 2_000)
report("Y - ticket + 40-page spec", 27_400, 300, 2_000)
report("Z - Y with the spec cached",27_400, 300, 2_000, cached=26_000)

# Expected output:
#
# X - ticket only  (1,400 in / 300 out x 2,000 calls)
#   tier              per call    per month     per year
#   A - frontier      0.043500        87.00      1044.00
#   B - workhorse     0.008700        17.40       208.80
#   C - small         0.000725         1.45        17.40
#
# Y - ticket + 40-page spec  (27,400 in / 300 out x 2,000 calls)
#   tier              per call    per month     per year
#   A - frontier      0.433500       867.00     10404.00
#   B - workhorse     0.086700       173.40      2080.80
#   C - small         0.007225        14.45       173.40
#
# Z - Y with the spec cached  (27,400 in / 300 out / 26,000 cached x 2,000 calls)
#   tier              per call    per month     per year
#   A - frontier      0.082500       165.00      1980.00
#   B - workhorse     0.016500        33.00       396.00
#   C - small         0.001375         2.75        33.00
```

**Stop and read X → Y.** Identical `calls`. Ten times the cost. **That is the session's insight, produced by your own code.**

Then read Y → Z on Tier B: `$173.40 → $33.00`, an **81 % reduction**, from caching a prefix that never changes.

---

## Part 4 — Watch a conversation go quadratic (5 min)

```python
# Cell 5
SYSTEM, USER_MSG, ASSISTANT_MSG = 500, 200, 400
TIER = TIERS[1]   # workhorse

def conversation_cost(n_turns):
    """Stateless API: at each turn we re-send the whole history."""
    history, cum_in, cum_out, cost = SYSTEM, 0, 0, 0.0
    rows = []
    for turn in range(1, n_turns + 1):
        turn_in = history + USER_MSG          # everything so far, plus the new message
        cum_in += turn_in
        cum_out += ASSISTANT_MSG
        cost += cost_per_call(TIER, turn_in, ASSISTANT_MSG)
        history = turn_in + ASSISTANT_MSG     # the reply joins the history too
        rows.append((turn, turn_in, cum_in, cost))
    return rows

naive_per_turn = cost_per_call(TIER, SYSTEM + USER_MSG, ASSISTANT_MSG)

print(f"{'turn':>5}{'input this turn':>17}{'cumulative input':>19}{'actual $':>11}{'naive $':>10}{'ratio':>8}")
for turn, turn_in, cum_in, cost in conversation_cost(20):
    if turn in (1, 2, 3, 5, 10, 20):
        naive = naive_per_turn * turn
        print(f"{turn:>5}{turn_in:>17,}{cum_in:>19,}{cost:>11.4f}{naive:>10.4f}{cost/naive:>8.2f}x")

# Expected output:
#  turn  input this turn   cumulative input   actual $   naive $   ratio
#     1              700                700     0.0081    0.0081   1.00x
#     2            1,300              2,000     0.0180    0.0162   1.11x
#     3            1,900              3,900     0.0297    0.0243   1.22x
#     5            3,100              9,500     0.0585    0.0405   1.44x
#    10            6,100             34,000     0.1620    0.0810   2.00x
#    20           12,100            128,000     0.5040    0.1620   3.11x
#
# Turn 20 costs 17x what turn 1 cost. Cumulative INPUT is 9x the naive estimate
# (128,000 vs 20 x 700 = 14,000).
```

Scale it: 1,000 twenty-turn conversations a month is **$504**, where a per-request forecast budgets **$162**.

---

## Part 5 — Price your own workload (5 min)

Replace the numbers with something from your actual job.

```python
# Cell 6 — EDIT THESE
my_prompt = """You are a release assistant. Summarise the ticket below in three
bullet points, propose a component owner, and draft two sentences for the
status page. Be concise: at most 100 words total.

TICKET:
"""  # ...paste a real (non-confidential) ticket after this

MY_INPUT_TOKENS  = count_tokens(my_prompt)   # measured, not guessed
MY_OUTPUT_TOKENS = 300                       # your realistic median answer length
MY_CALLS_MONTH   = 2_000                     # your real volume

report("MY WORKLOAD", MY_INPUT_TOKENS, MY_OUTPUT_TOKENS, MY_CALLS_MONTH)
```

Write down the Tier B monthly figure. Then answer three questions:

1. What fraction of the cost is **output**? (Compute it. Is "be concise" worth adding?)
2. What in your context is **stable across every call**? That is your cache candidate. Move it to the front.
3. If someone added a document, a retrieval step, or an agent loop to this, **how much would the request count change?** (Zero. That is the point.)

> ⚠️ **Do not paste confidential ticket text, customer data, or internal source into a public tool.** The tokenizer web page and Colab are both external services. Use a redacted or synthetic ticket. This is a live data-handling rule, not a lab formality — Session 14 covers it properly.

---

## Now break it / now extend it

**1. Break the cache (2 min).** In Part 3, model the mistake from `content/05` §4: put a timestamp *before* the stable prefix, so nothing can be cached. Set `cached=0` on workload Z and compare. You should get Y's numbers back — **$173.40**. One variable token in the wrong place costs 81 % of the saving.

**2. Find the crossover (5 min).** At what output length does output cost exceed input cost, for a 1,400-token input at Tier B? Solve it, then verify by looping. *(Answer: at a 5:1 price ratio, output overtakes input at 280 output tokens — one fifth of the input length. Most real answers are longer than that, which is why output usually dominates.)*

**3. Model the agent (5 min).** Write `agent_cost(steps, base_tokens=2000, step_growth=600, out_per_step=300)` that sums the per-step calls with a growing trace. Run it for 1, 4, 8 and 16 steps at Tier B. *(At 8 steps you should get ~$0.1344 — about 12.8× a single call. At 16 steps, notice it more than doubles again.)*

**4. Tokenise your own repository (5 min).** Point `count_tokens` at a real source file and at its README. Compare tokens-per-character. Then compute what it would cost to send the whole file to Tier A on every commit, at 200 commits a day. That number is why "add an LLM code review to CI" is a budget decision.

**5. Compare tokenisers (advanced).** Load `cl100k_base` alongside `o200k_base` and count the same five samples with both. The counts differ — sometimes by 10 %+. **Which one is "right"?** *(Neither. Each is right for its own model family. This is why you bill from the API's reported usage and not from your local estimate.)*

---

## What to take away

- A measured token count for **one real input from your own work**.
- A working estimator you can paste into a business case — with the prices marked as needing refresh.
- The reflex, when someone quotes a "cost per request," to ask: **"per request of what size, with what in the context?"**
