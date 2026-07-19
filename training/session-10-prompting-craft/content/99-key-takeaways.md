# Key Takeaways — Prompting I: The Craft

---

## The recap

**The cycle**
- Prompting is a loop: **define → draft → test → refine → iterate → evaluate**. The inner loop (test → refine one thing → test) is where the work happens.
- **Write the rubric before the prompt.** A checkable objective *is* your scoring function.
- **Change one thing per iteration**, or the result teaches you nothing.
- Know your **stopping rule** in advance — including the rule that says *the prompt is not the problem* (wrong model, undecomposed task, missing information).

**The taxonomy**
- Classify the task before writing: 11 practical types, four routing questions. It stops you applying one prompt shape to everything.
- **The Prompt Report v6** (CC BY 4.0) is the vocabulary authority when two people mean different things by "few-shot."

**Zero-shot and few-shot**
- Few-shot is **pattern continuation, not learning** — nothing in the model changes.
- 2–3 **hard, correctly-labelled, consistently-formatted** exemplars beat paragraphs of description.
- Few-shot fixes **shape**; it does not fix **knowledge**, and it actively suppresses **variety**.
- Exemplars are code: they can carry bugs (imbalance, copied errors) and they cost tokens on every call.

**Chain-of-thought**
- CoT works because **reasoning tokens buy computation**.
- **"Let's think step by step" is a 2023 artefact.** On modern models reasoning is a **parameter with a budget**, and the question is "is this call worth it?"
- Reasoning pays for **multi-step judgement** and is wasted on **pattern-matching**. It is not a correctness guarantee, and the visible chain is **not an audit trail**.
- **Self-consistency** (majority of n) and **decomposition** (split into testable calls) remain yours.

**Structure**
- **System message = standing policy** (and the cacheable part). **User message = this request and its data.**
- **Delimiters around any text you did not write**, named so you can refer to them. This is also the first line of defence against prompt injection — a substantial mitigation, not a fix (Session 14).
- The **grounding constraint** — *"describe only what is in the input"* — is the highest-value single line for transformational tasks.
- **Self-critique** catches rule violations well and factual errors badly. Two calls when a system acts on the output; one when a human reads it.

**Structured output**
- **Asking for JSON is a request; constrained decoding is a guarantee.** Know which you have.
- The **schema is part of the prompt** — enums, names and descriptions steer the model.
- **Always give a null/unknown branch.** A required field with no escape hatch is an instruction to fabricate.
- Add an **`evidence` field** of verbatim quotes; it is the cheapest verification you will ever get.
- Schema-valid ≠ true. Good formatting makes a wrong answer *harder* to spot, not easier.

**The cost lever**
- On narrow, well-specified, repetitive tasks, **prompt engineering substitutes for model spend** — often 10–20× cheaper, and faster.
- **Constraining output cuts cost**; output tokens are the expensive ones.
- **Static first, variable last** — cache-friendly ordering is the single biggest cost decision in prompt design.
- **Always ask "at equal token budget?"** Much of what looks like clever prompting is just more tokens.
- Report **pass rate, cost per call, tokens per call** together. Any two can tell a flattering story.

**Prompts as artifacts**
- A prompt is **production configuration**: in git, reviewed in a PR, tested in CI, its version logged on every call.
- **Start with five real cases**, grow to 20–50 harvested from actual failures, hold a fifth out.
- **Run the eval on a schedule** — the model can change underneath you.
- **Log human edits**; they are perfectly-labelled failure data most teams throw away.

---

## The corrections we carried

| The old advice | Why it changed |
|---|---|
| "Add *let's think step by step*" | Reasoning is now a model parameter with a token budget, not a magic phrase |
| "Ask nicely for JSON" | Constrained decoding makes schema-validity a guarantee, not a hope |
| "Testing is the last step" | Testing is the *precondition* — without success criteria and a way to check them, you are guessing |
| "Prompt engineering is a list of tricks" | The tricks expire on an 18-month cycle; the **loop** does not |
| Source deck: a "Cookbook" with no recipes | Every technique here ships with a complete, verbatim prompt |

---

## The self-test

If you can do these five things, the session worked:

1. Write a checkable objective for one of your own tasks — one a colleague could grade against without asking you a question.
2. Build a 5-case eval set from real inputs, including the empty case and the messy case.
3. Take a prompt from zero-shot to system-message + delimiters + few-shot + output contract, and say what each element defends against.
4. Get schema-valid JSON out of a model, with a null branch and an evidence field.
5. Run a small model against a large one on your eval set and report **pass rate, cost per call, and tokens per call**.

---

## If you remember one thing

> **A prompt you have not tested is folklore. Write down what "good" means, collect five real inputs that have burned you, change one thing at a time, and measure — because these nuances are found through testing, not guessing.**
