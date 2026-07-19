# Lab — Build an Eval Set and A/B Two Prompts

**Time:** ~25–30 minutes · **Environment:** Google Colab (JupyterLite fallback below) · **Language:** Python
**You will finish with:** a 6-case eval set, two prompt versions scored against it, and a table showing pass rate, cost per call, and tokens per call for each.

This is the lab that makes `content/01`, `content/07`, and `content/08` concrete. It is deliberately small — the point is that the whole loop fits in one notebook, not that the harness is sophisticated.

---

## Setup

### Colab (recommended)

1. Open `colab.research.google.com` → **New notebook**.
2. Install one SDK:

```python
!pip install -q anthropic
# or:  !pip install -q openai
```

3. Provide a key. In Colab use the key icon in the left sidebar (**Secrets**), add `ANTHROPIC_API_KEY`, enable notebook access — **do not paste a key into a cell**, notebooks get shared.

```python
import os
from google.colab import userdata
os.environ["ANTHROPIC_API_KEY"] = userdata.get("ANTHROPIC_API_KEY")
```

4. Set your model IDs. **⚠️ These are placeholders — get the current small and large model IDs from your provider's documentation before running.**

```python
SMALL_MODEL = "claude-<small-model>-<version>"   # VERIFY AT DELIVERY
LARGE_MODEL = "claude-<large-model>-<version>"   # VERIFY AT DELIVERY

# Prices in USD per 1M tokens - VERIFY AT DELIVERY
PRICES = {
    SMALL_MODEL: {"in": 0.80, "out": 4.00},
    LARGE_MODEL: {"in": 15.00, "out": 75.00},
}
```

### No API key? Two fallbacks

- **JupyterLite** (`jupyter.org/try-jupyter/lab/`) runs in-browser but has no network access to model APIs. Use it for Steps 1, 2 and 5 (writing cases, writing prompts, reading the scoring logic) and pair with someone who has a key for Steps 3–4.
- **Manual mode:** run the two prompts by hand in whatever chat tool you have, paste the six outputs into the `MANUAL_OUTPUTS` dict provided in Step 4, and let the scorer grade them. You lose the token/cost columns but you get the pass-rate comparison, which is the main lesson.

---

## Step 1 — The fixtures (3 min)

Six realistic commit logs. Copy this cell as-is.

```python
FIXTURES = {
"normal": """
feat(camera): add night mode toggle to the capture UI (PLT-8891)
fix(audio): resolve crackling on BT headsets during network handover
chore(deps): bump protobuf 3.21.9 -> 3.21.12
refactor: extract RadioStateMachine into its own module
fix(ui): correct battery icon alignment in landscape on tablets
test: add regression coverage for handover path
feat(power): reduce idle drain by deferring background sync (PLT-9012)
""",

"internal_only": """
refactor: split MediaController into three collaborators
chore(ci): pin the build image to a digest
test: flake fix in HandoverIntegrationTest
chore(deps): bump junit 5.10.1 -> 5.10.2
docs: update the module README
""",

"single_feature": """
feat(connectivity): support Wi-Fi 7 MLO on supported chipsets (PLT-7734)
""",

"vague": """
fix: various fixes
update stuff
misc
wip
address review comments
""",

"truncated": """
feat(camera): add night mode toggle to the cap
fix(audio): resolve crackling on BT headse
""",

"large": "\n".join(
    [f"fix(module{i}): correct off-by-one in buffer sizing (PLT-{7000+i})" for i in range(18)]
    + [f"feat(module{i}): expose new configuration flag for retry backoff" for i in range(6)]
),
}
```

Note what these are: the normal case, the empty case, the trivial case, the *vague* case (tests the grounding constraint), the *truncated* case (tests graceful degradation), and the *oversized* case (tests the ≤12-bullet rule). This is the harvest pattern from `content/08` — the awkward ones do the work.

---

## Step 2 — The two prompts (2 min)

```python
PROMPT_V1 = "Write release notes from these commits."

PROMPT_V6 = """You draft release notes for a mobile platform, for an external
audience of customers and integration partners.

Output contract:
- A Markdown bullet list, nothing else. No preamble, no closing remarks.
- One bullet per user-visible change.
- Each bullet starts with a past-tense verb and is at most 20 words.
- Plain language. No internal component code-names, no ticket IDs.
- Maximum 12 bullets. If more than 12 user-visible changes exist, merge the
  smallest related ones.
- If no change in the input is user-visible, output exactly:
  No user-visible changes.

Exclusions - omit entirely:
- Pure refactors with no behaviour change
- Test-only changes
- Dependency bumps with no user-visible effect
- CI, build, and tooling changes

Grounding:
- Every bullet must be traceable to at least one line of the supplied commit
  log. Do not describe changes that are not present in the input.
- If a commit message is too vague to describe a user-visible effect, omit it
  rather than guessing what it did.

Examples:
  Commit: "fix(audio): resolve crackling on BT headsets during handover"
  Bullet: "- Fixed audio crackling on Bluetooth headsets during network handover."

  Commit: "chore(deps): bump protobuf 3.21.9 -> 3.21.12"
  Bullet: (omitted - dependency bump with no user-visible effect)

  Commit: "refactor: extract RadioStateMachine into its own module"
  Bullet: (omitted - pure refactor)"""
```

---

## Step 3 — The eval set and the scorer (5 min)

```python
import re

CASES = [
    {"name": "normal",         "fixture": "normal",
     "not_contains": ["PLT-", "protobuf", "RadioStateMachine"], "max_bullets": 12},
    {"name": "internal_only",  "fixture": "internal_only",
     "equals": "No user-visible changes."},
    {"name": "single_feature", "fixture": "single_feature",
     "not_contains": ["PLT-"], "max_bullets": 1},
    {"name": "vague",          "fixture": "vague",
     "max_bullets": 2},   # should omit almost everything - grounding test
    {"name": "truncated",      "fixture": "truncated",
     "not_contains": ["I'm sorry", "As an AI", "appears to be cut off"], "max_bullets": 2},
    {"name": "large",          "fixture": "large",
     "max_bullets": 12},  # the hard cap
]

def bullets(text: str) -> list:
    return [l for l in text.splitlines() if l.strip().startswith("-")]

def score(case: dict, out: str) -> tuple:
    """Return (passed, list_of_violations). Dumb, deterministic, sufficient."""
    v = []
    out = out.strip()
    if "equals" in case and out != case["equals"]:
        v.append(f"expected exact string, got {out[:50]!r}")
    for bad in case.get("not_contains", []):
        if bad in out:
            v.append(f"contains forbidden {bad!r}")
    if "max_bullets" in case:
        n = len(bullets(out))
        if n > case["max_bullets"]:
            v.append(f"{n} bullets, max {case['max_bullets']}")
    for b in bullets(out):
        if len(b.split()) > 22:          # 20 words + "- " tolerance
            v.append(f"bullet too long: {b[:40]}...")
            break
    return (len(v) == 0, v)
```

Read the scorer before running it. Every check is a string operation. **No LLM judge, no embedding similarity, no framework.** That is not a simplification for teaching — it is what most useful prompt regressions actually look like.

---

## Step 4 — Run the A/B (8 min)

```python
from anthropic import Anthropic
client = Anthropic()

def run(model: str, system: str, commits: str) -> tuple:
    resp = client.messages.create(
        model=model, max_tokens=800, temperature=0,
        system=system,
        messages=[{"role": "user", "content": f"<commits>\n{commits}\n</commits>"}],
    )
    return resp.content[0].text, resp.usage

def evaluate(label: str, model: str, system: str) -> dict:
    passes = tin = tout = 0
    print(f"\n=== {label} ===")
    for case in CASES:
        out, usage = run(model, system, FIXTURES[case["fixture"]])
        ok, violations = score(case, out)
        passes += ok
        tin += usage.input_tokens
        tout += usage.output_tokens
        print(f"  {'PASS' if ok else 'FAIL'}  {case['name']:<16} "
              f"{'' if ok else violations[0]}")
    p = PRICES[model]
    cost = (tin / 1e6) * p["in"] + (tout / 1e6) * p["out"]
    return {"label": label, "pass_rate": passes / len(CASES),
            "cost_per_call": cost / len(CASES),
            "tokens_per_call": (tin + tout) / len(CASES)}

results = [
    evaluate("small + v1 (lazy)",       SMALL_MODEL, PROMPT_V1),
    evaluate("small + v6 (engineered)", SMALL_MODEL, PROMPT_V6),
    evaluate("large + v1 (lazy)",       LARGE_MODEL, PROMPT_V1),
]

print(f"\n{'config':<26} {'pass':>6} {'$/call':>10} {'tok/call':>10}")
for r in results:
    print(f"{r['label']:<26} {r['pass_rate']:>5.0%} "
          f"{r['cost_per_call']:>10.4f} {r['tokens_per_call']:>10.0f}")
```

**Expected output (illustrative — your numbers will differ, and that is the point):**

```
=== small + v1 (lazy) ===
  FAIL  normal           contains forbidden 'PLT-'
  FAIL  internal_only    expected exact string, got '- Refactored the MediaC'
  FAIL  single_feature   contains forbidden 'PLT-'
  FAIL  vague            5 bullets, max 2
  FAIL  truncated        contains forbidden 'appears to be cut off'
  FAIL  large            24 bullets, max 12

=== small + v6 (engineered) ===
  PASS  normal
  PASS  internal_only
  PASS  single_feature
  PASS  vague
  PASS  truncated
  FAIL  large            14 bullets, max 12

=== large + v1 (lazy) ===
  FAIL  normal           contains forbidden 'PLT-'
  FAIL  internal_only    expected exact string, got 'Based on the commits pr'
  PASS  single_feature
  FAIL  vague            4 bullets, max 2
  PASS  truncated
  FAIL  large            19 bullets, max 12

config                       pass     $/call   tok/call
small + v1 (lazy)              0%     0.0009        320
small + v6 (engineered)       83%     0.0021        890
large + v1 (lazy)             33%     0.0165        340
```

### Read the result carefully — this is the lab's whole point

- **The expensive model with a lazy prompt loses to the cheap model with a good one**, by 50 percentage points, at roughly 8× the cost per call.
- **The engineered prompt costs more per call than the lazy one on the same model** (more input tokens). It is still an order of magnitude cheaper than the frontier model, and it is the only configuration that works.
- **Reporting all three columns is what keeps this honest.** Pass rate alone flatters the engineered prompt; cost alone flatters the lazy one; tokens alone shows you bought part of the improvement with spend. You need all three to make a defensible claim — this is the "at equal token budget?" discipline from `content/07`, applied to your own work.
- **A remaining FAIL is a good outcome.** The `large` case failing on the bullet cap tells you exactly what v7 must fix. An eval set that passes 100% on the first try is measuring nothing.

---

## Step 5 — Fix the remaining failure (5 min)

Write `PROMPT_V7`: take v6 and change **one** thing to fix the `large` case. Suggestions, pick one:

- Make the cap more emphatic and give the merge rule a worked example.
- Add a negative exemplar showing two related fixes merged into one bullet.
- Decompose: ask for a bulleted draft, then a second call that merges down to 12.

Re-run `evaluate("small + v7", SMALL_MODEL, PROMPT_V7)`. **Did it fix `large` without breaking anything else?** That question — regression, not improvement — is the one that separates a prompt engineer from a prompt tweaker.

---

## Now break it / now extend it

**1. Break the exemplars.** In `PROMPT_V6`, change the third exemplar so the refactor commit is *included* as a bullet instead of omitted. Re-run. You should see refactors reappear in the output on multiple cases. The lesson: **an incorrect exemplar is a bug that ships, and the model copies it with perfect fidelity.** Review exemplars like code.

**2. Break the cache-friendliness.** Move the exemplars from the system message into the user message, after the commit log. Functionally identical prompt. Now imagine 50,000 calls a day: the static prefix is gone, so nothing is cacheable. If your provider reports cache metrics, run it 20 times each way and compare. This is `content/07`'s ordering rule made visible.

**3. Add structured output.** Change the task to return JSON: `{"bullets": [...], "omitted_count": int, "had_user_visible_changes": bool}` using a schema with `strict`/tool-forcing per `content/06`. Then rewrite the scorer to check fields instead of parsing text. Notice how much simpler the scorer becomes — **structured output is as much a testability feature as a pipeline feature.**

**4. Add a reasoning comparison.** Run the same eval with reasoning enabled on the small model. Record pass rate, cost, tokens, and latency. On this task — a transformational task with an explicit contract — expect little or no accuracy gain for a substantial cost and latency increase. Verifying that yourself is worth more than the slide that told you.

**5. Turn it into CI.** Move `CASES` into a YAML file, the prompts into `prompts/*.txt`, write the pass rate to `baseline.json`, and wrap Step 4 in a script that exits non-zero on regression. You are now at level 4 of the maturity ladder in `content/08`, and it took about twenty minutes.

---

## What you should be able to say afterwards

- "I have run the prompt-engineering cycle end to end on a real task."
- "I have seen a cheap model with a good prompt beat an expensive model with a bad one, and I have the three numbers to prove it."
- "I know what my eval set's assertions look like, and they are mostly string checks."
- "I know why the remaining failure is useful."
