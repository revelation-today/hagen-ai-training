# Self-Check Quiz — Session 11

Ten questions. Answers and explanations at the bottom. If you can answer 1, 4, 6, and 9 without looking, you have the session.

---

### 1.
A release-notes prompt produces output with three defects: an invented version number, an internal refactor described as a customer feature, and two changes silently dropped. Which is the most dangerous, and why?

### 2.
Name the two single lines that do the most work in the production release-notes prompt from `content/01`, and state the one root cause they both address.

### 3.
You ask for a config review and get: *"This change appears generally safe, with some considerations…"* — despite the change including `tls_verify: true -> false`. Diagnose the failure using the five categories. There are two.

### 4.
A colleague's prompt fails. They rewrite it substantially, switch to a more capable model, and add three constraints, all at once. It now works. What is wrong with this, in engineering terms?

### 5.
Why must a prompt test case assert on *properties* rather than on an exact expected output? Give one property assertion and one bad assertion for a release-notes prompt.

### 6.
Your suite reports: v2 on the expensive model = 14/20 at $0.031/run; v3 on the cheap model = 16/20 at $0.004/run. What is the headline conclusion, and what is the *caveat* that stops you from simply switching everything to the cheap model?

### 7.
Someone re-pastes the same three paragraphs of background into a new chat every day and reports that the model is "inconsistent." What is actually happening, and what is the fix?

### 8.
Extended thinking fixes exactly one of the five diagnosis categories. Which one — and name two things people wrongly expect it to fix.

### 9.
In MCP, what is the difference between a tool and a resource, and where must access restrictions be enforced?

### 10.
Give one example each of: a task where Claude is a good fit, a task where it is a poor fit, and a task where it is a good fit *only* because the output is verifiable.

---
---

## Answer key

### 1.
**The silent omission.** The first two are visible — anyone proofreading catches an invented version number or a mis-framed refactor. A dropped change leaves no trace in the output; the only way to detect it is to compare against the source, which nobody does when the document reads well. The general principle: **design against the errors you cannot see, not the ones you can.** The structural fix is requiring an explicit "Omitted, and why" section, which converts an invisible failure into a visible one.

### 2.
`Do NOT invent a version number, a release date, or a severity that is not present in <changes>` and the escape hatch: `If an entry is too ambiguous to classify confidently, put it under "Needs author review"… Do not guess.`

**The shared root cause:** a model completing a pattern will fill a slot the pattern says should be filled, whether or not it has the information. Naming the slots to leave empty, and providing somewhere to put uncertainty, converts silent fabrication into a visible flag. Note also what does *not* work: "be accurate" or "be careful". Exhortation is not a control.

### 3.
**Category 2, ambiguous task** — "safe" was never defined, so the model chose a frame (performance tuning) and answered coherently inside it. **Category 1, missing context** — nothing told it what the system is, what it carries, or where it runs, so it had no basis to weight a TLS change against a batch-size change.

Worth adding: four changes, three benign, one serious, and the serious one was averaged into the tone of its neighbours. A model asked for an aggregate verdict produces an aggregate verdict; the structural fix is a per-item verdict before any overall one.

### 4.
They changed four things simultaneously, so they have learned nothing about which one mattered, cannot reproduce the fix, and are now paying permanently for a more capable model that may have been unnecessary. They may also have papered over an ambiguous instruction that is still there, waiting to fail when the input shape changes.

The discipline: **one change per pass, same input every pass**, diagnose before treating.

### 5.
The model is stochastic and prose has many valid forms — asserting exact output means the test fails on differences that do not matter, and you spend your life updating expected strings until you stop trusting the suite.

- **Good:** `output must not match \d+\.\d+\.\d+` (no invented version number); `output must contain "HEL-5019"`; `output has at most 400 words`.
- **Bad:** `output equals "## Fixed\n- [HEL-5011] …"` — brittle and uninformative. Also bad: `output is professional` — not checkable, even by a judge, without a sharper rubric.

### 6.
**Headline:** the prompt was worth more than the model. A better prompt on a cheaper model beat a worse prompt on an expensive one, at roughly one-eighth the cost — and you would never have known without the suite.

**The caveat:** look at *which* cases fail, not just the totals. In the worked example the cheap model failed every adversarial case. So the defensible policy is split, not blanket: cheap model where a human reviews the output, expensive model where output publishes unreviewed. Aggregate pass rates conceal exactly this kind of structure.

### 7.
They are writing a slightly different prompt every day and have not noticed. The model is not inconsistent; its input is. Retyped-from-memory context drifts, and a prompt that drifts **cannot be improved — only varied.**

**Fix:** move the stable context somewhere stable — a Project, a system prompt, or at minimum a text file they paste from. This usually produces a bigger quality improvement than any prompting technique, because it makes the prompt an object that *can* be improved.

### 8.
**Category 4, capability/model mismatch** — tasks that need multi-step reasoning, holding several interacting constraints together, or tracing consequences.

It does **not** fix: missing context (category 1) — it will reason beautifully about information you failed to supply; ambiguous instructions (category 2) — it reasons hard about the wrong question; unconstrained output (category 3); and it does not add knowledge or prevent hallucination. Also: more budget is not safer — cost and latency rise roughly linearly while accuracy flattens. Find the flattening point by measurement.

### 9.
**Resources are read; tools act.** A resource is content the client fetches into context, addressed by URI — closer to a file than a function. A tool is an action the model invokes, with arguments and consequences. The practical rule: **if it has a side effect it is a tool, and it needs an authorisation story.**

**Restrictions are enforced in the server** — in your code — never in the system prompt and never in the model's discretion. Anything you rely on must live somewhere it cannot be argued with. (And a server exposing write-capable tools needs the full Session 14 treatment before it goes anywhere near production.)

### 10.
Answers vary; a good one shows the reasoning, not the example.

- **Good fit:** drafting release notes from a change list — repetitive, structured, human reviews before publication, and the source of truth is in the prompt.
- **Poor fit:** "is this config value consistent with what we deployed to the EU region last Thursday?" — the model has no access to that record, so no prompt fixes it. Get the record in, or use a different tool.
- **Good fit only because verifiable:** an incident summary using the ESTABLISHED / INFERRED / UNKNOWN split — genuinely useful, and defensible *only* because the structure lets a human check nine specific lines against the timeline in ten minutes. Remove that structure and the same task becomes a fluent, confident, unverifiable narrative — which is worse than no summary, because it becomes the thing everyone cites.

---

## Scoring

| Score | Reading |
|---|---|
| 9–10 | You have it. Go build the test set — `exercises/lab.md` |
| 6–8 | Solid. Re-read `content/02` (diagnosis) and `content/03` (test sets) |
| 3–5 | Re-read `content/00` and `content/99`, then work back through the worked examples in `content/01` |
| 0–2 | Start with `content/99-key-takeaways.md`, then read the whole of `content/` in order |
