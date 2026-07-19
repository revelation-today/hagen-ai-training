# Key Takeaways — Session 14

## The ten things worth keeping

1. **There is no boundary between instruction and data.** A language model receives your system prompt and the untrusted document as one token stream. Everything else in this session follows from that sentence.

2. **Prompt injection has no clean fix.** SQL injection was solved by parameterisation — giving the database a way to tell code from data. No equivalent exists for language models, because for a language model everything is language. Mitigations reduce probability; none is a boundary.

3. **Indirect injection is the form that matters.** The attacker never touches your system. They put instructions in a defect record, a commit message, a vendor datasheet, a wiki page, a log line — and wait for your pipeline to read it.

4. **Three preconditions make a system dangerous:** untrusted content + private data + an outbound channel. Remove any one leg and the exfiltration path breaks. Run this test on every design. *(Framing after Simon Willison.)*

5. **Jailbreaking cannot be fully prevented**, because safety is trained rather than enforced, the input space is unbounded and adversarially searchable, the dual-use boundary is genuinely fuzzy, and every guardrail is itself a probabilistic model. Design so a successful jailbreak is disappointing rather than expensive.

6. **Four leakage paths, four controls:** transit/storage (contract), training use (verify per tier), context (least privilege — anything in the window is reachable by anything in the window), output (validation and egress). Default to RED when unsure, and remember that an embedding is not anonymisation.

7. **The OWASP Top 10 for LLM Applications 2025 is your checklist** — and it is CC BY-SA 4.0, so you may actually use it. The two new entries name what changed in deployment: **LLM07 System Prompt Leakage** (treat the system prompt as public) and **LLM08 Vector & Embedding Weaknesses** (your RAG index is an attack surface).

8. **Shrink the triangle.** Every hazard is a Hazard Source, an Initiating Mechanism, and a Target/Threat Outcome. Reduce any one and risk falls; eliminate any one and it collapses. For LLMs the hazard source is always the same thing — **a piece of wrong information** — which is why hallucination, injection, jailbreaks and poisoning all yield to one method.

9. **No automated pipeline acts on model output without a qualified human gate.** All three words matter: *automated* (unattended action is the danger), *qualified* (a reviewer who cannot catch the error is a rubber stamp that also creates false confidence), *gate* (it must be able to stop the action, and stopping is the default).

10. **Compiles and works ≠ secure.** ~40% of security-relevant Copilot suggestions carried a vulnerability in 2021; independent studies across modern models in 2024–25 land in the 45–62% range. Four years, better models, no improvement. Review — not authorship — is now the bottleneck.

---

## The two rules that change behaviour

Everything above compresses into these. If the room leaves with nothing else:

> **1. Assume the model will be told to do something you did not tell it to do. Design for that being true.**
>
> **2. You make a system safer by constraining it to do less.**

---

## If you remember one thing

> **Put the security boundary somewhere a parser can enforce it — in permissions, in network egress, in a human approval step. Never in the wording of a prompt.**

---

## What you should have in hand

| Artefact | From | Use it for |
|---|---|---|
| The four-tier paste rule | `03` §2 | Publish with your real examples, one page |
| The OWASP review-gate table | `04` §3 | Attach to every proposed AI use |
| The operating-domain template | `05` §2 | Fill one in per deployment; the *barred* rows are the valuable ones |
| The nine-section policy skeleton | `08` §3 | The team's own AI-use policy, drafted from NIST + UK Playbook |
| The Gandalf experience | `exercises/lab.md` | The argument you make when someone says "we'll just put a guardrail on it" |

---

## Where this connects

| Session | Link |
|---|---|
| **1** | "Pattern-matcher, not search engine" is *why* there is no instruction/data boundary |
| **9** | Tokens are the unit — the reason the model sees one stream and not two channels |
| **11** | Tools and connectors are exactly what turns a text failure into an action failure |
| **12** | Hallucination is the *non-adversarial* source of the same hazard; the 99%/1% problem is why the human gate degrades as the model improves |
| **14** | The judgement, verification and accountability work this session creates is the work that does not automate away |

---

## Two honest caveats to carry out of the room

**This session is a floor, not a ceiling.** A checklist tells you which known categories to consider. It does not tell you whether your particular use is worth the risk — that judgement is yours, and `05` is the method for making it.

**Every number and date here drifts.** OWASP editions, insecure-code percentages, vendor retention terms, and above all the EU AI Act timeline (the 2027-12-02 high-risk deferral is **provisional**). Re-verify before you rely on any of it. See `resources/sources.md`.
