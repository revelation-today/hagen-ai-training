# Exercise — The Role Self-Audit

**No hands-on lab — this is a concept and discussion session.** In its place, this is the take-home exercise, and it is the most useful thing a participant does with this session. Roughly **25 minutes**, done alone, on your own real job.

There is one optional Python snippet at the end for anyone who prefers to compute the summary rather than eyeball it. It is genuinely optional; paper works fine.

---

## Why do this

The four role sections in `content/` are generic by necessity. Your actual week is not generic. The audit converts the session's framework into a picture of *your* job, and it produces two things worth having:

- A defensible answer to "which parts of my work are exposed?" — based on evidence rather than mood.
- A **catch log**, which is the single most effective defensive act available to you (`content/10` §2, skill 3).

---

## Step 1 — List your sub-tasks (8 min)

Open your calendar and your ticket queue for the **last two working weeks** — not a typical week you're imagining, the actual last two.

Write down every distinct sub-task you performed. Aim for **15–25 items**. Be granular: not "managed the release" but "chased three teams for sign-off," "wrote the release notes," "ran the go/no-go," "argued for a one-day slip."

Estimate hours for each. Rough is fine; the ordering matters more than the precision.

> **Common mistake:** writing job responsibilities instead of sub-tasks. If an item could appear on a job description, it is too coarse. Split it.

## Step 2 — Bucket each one (7 min)

For each sub-task, assign one bucket, using the decision rule from `content/10` §1:

| Bucket | Test |
|---|---|
| **A — Automatable** | A competent AI could produce this today, and I could verify it in a fraction of the time it takes me to produce it. |
| **G — Augmentable** | AI could draft it or propose options; I would still make the call and own the result. |
| **H — Human-only** | Judgement, authority, negotiation, ground truth, or accountability. AI contributes nothing I could safely ship. |
| **D — Deterministic** | This needs a *guarantee*. The right tool is a checker, a diff, a validator, or a test — not an AI, and possibly not me either. |

Be honest on **A**. The instinct is to under-assign it, because doing so feels safer. It isn't — under-assigning A means you will be surprised by a change you could have anticipated.

## Step 3 — Mark what gets harder (5 min)

Go back through the list and mark **+** against any sub-task that has become *harder* in the last year, for any of these reasons:

- The input you receive is now AI-drafted (longer, more fluent, harder to judge)
- The volume went up without the time going up
- You are now verifying output that is *usually* right — the vigilance problem

This column is the one nobody plans for and it is where your next twelve months actually live.

## Step 4 — Read the result (5 min)

Total the hours per bucket and answer four questions in writing. Writing, not thinking — the sentences are the exercise.

1. **What share of my time is bucket A?** If it is over 40%, your role's composition will change materially and soon. That is information, not a verdict.
2. **Of my bucket-A hours, how many would I actually miss?** Usually few. This is the genuinely encouraging number and it is more honest than any reassurance.
3. **What is my largest H item, and is it visible to anyone else?** If the highest-value thing you do leaves no written trace, fix that first.
4. **What is my largest + item, and what would make it manageable?** Deterministic gates in front of review? A different review criterion? More time, requested with numbers? Pick one and name it.

---

## Optional — compute the summary in Python

```python
# Role self-audit summary. Buckets: A=automatable, G=augmentable,
# H=human-only, D=needs a deterministic tool. "+" = got harder.
tasks = [
    # (description, hours, bucket, got_harder)
    ("Draft release notes",              3.0, "A", False),
    ("Chase sign-offs across teams",     4.0, "G", True),
    ("Review AI-drafted change records", 5.0, "H", True),
    ("Run go/no-go",                     2.0, "H", False),
    ("Negotiate a one-day slip",         1.5, "H", False),
    ("Verify build matches baseline",    2.0, "D", False),
]

total = sum(h for _, h, _, _ in tasks)
by_bucket = {}
for _, hours, bucket, _ in tasks:
    by_bucket[bucket] = by_bucket.get(bucket, 0.0) + hours

for bucket in ("A", "G", "H", "D"):
    hours = by_bucket.get(bucket, 0.0)
    print(f"{bucket}: {hours:5.1f} h  ({hours / total:5.1%})")

harder = sum(h for _, h, _, flag in tasks if flag)
print(f"Got harder: {harder:.1f} h ({harder / total:.1%}) — plan for this share.")

# Expected output for the sample data above:
# A:   3.0 h  (17.6%)
# G:   4.0 h  (23.5%)
# H:   8.5 h  (50.0%)
# D:   2.0 h  (11.8%)
# Got harder: 9.0 h (52.9%) — plan for this share.
```

Replace the `tasks` list with your own. The "got harder" percentage is usually the number that surprises people.

---

## Step 5 — Start the catch log (ongoing, 2 min/week)

A single file or note. Every time your review, your judgement, or your question catches something a tool would not have:

| Date | What I caught | What it would have cost |
|---|---|---|
| 2026-07-14 | AI-drafted release notes omitted a breaking API change | Customer escalation, unplanned patch |
| 2026-07-16 | Proposed root cause was plausible; evidence pointed elsewhere | Wrong corrective action, problem recurs |

Two minutes a week. After one quarter you have a document that answers "what does this verification step actually catch?" with evidence rather than assertion — which is the argument that decides whether verification gets staffed (`content/04` §6).

---

## Bring to the follow-up

- Your bucket percentages (no need to share task detail).
- Your largest **H** item and whether it is visible.
- One thing from Prompt 4 in `discussion.md` — a genuinely valuable use nobody has tried because it wasn't worth a human's time before.
