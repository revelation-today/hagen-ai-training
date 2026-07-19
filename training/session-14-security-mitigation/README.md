# Session 14 — Risk II: Security, Privacy & Mitigation in Practice

**Block:** Use it responsibly · **Goal covered:** 7 (second half) · **Format:** 45 min content + 15 min Q&A

---

## One-paragraph summary

Session 13 covered the failure modes that come from *what the technology is* — it is confidently wrong because nothing in the mechanism checks a pattern against truth. This session covers the failure modes that come from *how you deploy it*. The centrepiece is **prompt injection**: an LLM cannot reliably tell the difference between the instructions you gave it and the text it is asked to process, because both arrive as one undifferentiated stream of tokens. There is no equivalent of a prepared statement. That is not a bug awaiting a patch; it is a property of the architecture, and it gets sharply worse the moment an LLM is wired to *act* — to call tools, hit APIs, or feed an automated pipeline. Around that core we put four practical things the room can use on Monday: a **privacy discipline** for config and release data, the **OWASP Top 10 for LLM Applications 2025** as a working checklist, the source deck's genuinely good **HS/IM/TTO hazard triangle and operating-domain** method, and the one hard number about AI-assisted coding — *compiles and works ≠ secure*. The session ends by handing the team a policy-drafting task built on public-domain templates, and one honest slide on what the EU AI Act actually requires of a company that **uses** AI rather than builds it.

## Audience & level

Qualcomm release / problem / configuration managers and developers. This is the most security-flavoured session in the series but it is **not** a security-engineering course: no exploit development, no red-team tooling installation. Managers get the risk model, the checklist, and the policy work; developers additionally get two Python patterns (a trust-boundary sketch and an input/output validation sketch) they can lift into real code. Everyone participates in the live Gandalf activity — it needs a browser and nothing else.

Framing note, stated up front in the room: this is **defensive** material. We look at how these systems break so we can gate them properly. Nothing here is an attack recipe, and the payloads used are deliberately trivial and public.

## Learning objectives

After this session a participant can:

1. **Explain** why prompt injection has no clean fix, using the "one token stream, no instruction/data boundary" argument, and contrast it with SQL injection where parameterisation *does* solve the problem.
2. **Distinguish** direct from indirect prompt injection, and identify which one applies to a RAG pipeline, a document summariser, or an agent that reads a ticket queue.
3. **Apply** the three-precondition test for a dangerous agent — untrusted input + private data + an outbound channel — and name which leg to remove in a given design. *(Framing after Simon Willison; concept paraphrased, prose not reproduced.)*
4. **Decide** what may and may not be pasted into a third-party model, for the specific categories of data this team handles: config baselines, release notes, defect records, logs, customer identifiers.
5. **Use** the OWASP Top 10 for LLM Applications 2025 as a review checklist against a proposed internal AI use, and say what LLM07 (System Prompt Leakage) and LLM08 (Vector & Embedding Weaknesses) add over the 2023 edition.
6. **Map** a proposed LLM use onto the HS/IM/TTO hazard triangle and shrink it by constraining the operating domain — including the hard rule that no automated pipeline acts on model output without a qualified human gate.
7. **State** the deployer-side EU AI Act position: AI literacy live now, transparency from 2026-08-02, high-risk duties provisionally deferred — and flag it as provisional.
8. **Draft** one section of an internal AI-use policy using the NIST AI RMF Playbook and the UK Government AI Playbook as source material.

## Prerequisites

- **Session 13 (Risk I)** — hallucination, human-in-the-loop being necessary but not sufficient, the 99%/1% detection problem. This session assumes it.
- **Session 1** — "autocomplete on steroids; a pattern-matcher, not a search engine." The injection argument depends on it.
- Helpful but not required: **Session 11** (working with Claude, tools/connectors) and **Session 9** (tokens as the unit the model actually sees).
- No security background assumed. Developers who know SQL injection will get an extra beat from the comparison.

## Agenda (45 min + 15 min Q&A)

| Time | Segment | What happens |
|---|---|---|
| 0–5 min | **Hook — everyone breaks a model** | Lakera Gandalf, levels 1–3, live on phones/laptops. Zero setup. The room defeats a guarded LLM inside four minutes. |
| 5–12 min | **Prompt injection: no boundary, no fix** | One token stream. Direct vs. indirect. Why "just tell it to ignore instructions in the document" is not a control. |
| 12–18 min | **The three preconditions** | Untrusted content + private data + an outbound channel. Why agents and automated pipelines convert a nuisance into an incident. |
| 18–24 min | **Jailbreaking and the limits of guardrails** | Gandalf debrief: the later levels are guarded and *still* fall. Why full prevention is unsolved. |
| 24–30 min | **Data leakage: what not to paste** | Retention, training use, PII — applied to config baselines, defect records, logs, release data. |
| 30–35 min | **Insecure code generation** | ~40% in 2021; **45–62% in 2024–25**. It did not improve. *Compiles and works ≠ secure.* |
| 35–40 min | **The checklist: OWASP LLM Top 10 2025** | All ten, and the two new entries. This is the slide people photograph. |
| 40–44 min | **The method: hazard triangle + operating domain** | HS/IM/TTO; shrink the triangle; constrain to do less; the human gate rule. |
| 44–45 min | **EU AI Act + your homework** | Sixty honest seconds on deployer duties, then the policy-drafting assignment. |
| 45–60 min | **Q&A / policy kickoff** | See `exercises/discussion.md`. Optionally run the full 15-min Gandalf lab here instead. |

**Is 45 minutes honest?** It is **tight** — this is the densest session in the series and it carries the most authored material. Hold the discipline, and if you run long cut in this order: (1) the EU AI Act slide down to a single sentence and a pointer to `content/07`; (2) the jailbreak-taxonomy slide, keeping only the Gandalf debrief; (3) the OWASP walkthrough down to "the four that apply to us" rather than all ten. Do **not** cut the Gandalf hook — it is what makes the rest land — and do not cut the human-gate rule, which is the one instruction that changes behaviour.

**Alternative shape (recommended if you have 60 min of content time):** move the full 15-minute Gandalf lab into its own block and run the session as 60 + 15.

## Materials & tools

| Item | Detail |
|---|---|
| **Live interactive** | **Lakera Gandalf** — `https://gandalf.lakera.ai/` — browser only, no signup, no install. Presenter should have completed levels 1–4 beforehand. **Live demo only — do not screenshot into the deck** (proprietary). Have a text-described fallback ready in case of no network. |
| **Slides** | `slides/outline.md` → built per `../powerpoint_instructions.md`. |
| **Lab** | `exercises/lab.md` — the structured 15-min Gandalf activity with debrief, plus two optional Python parts (trust-boundary sketch, input/output validation) runnable in Colab. |
| **Checklist handout** | The OWASP LLM Top 10 2025 table from `content/04` — CC BY-SA 4.0, printable, attribution line required. |
| **Policy templates** | NIST AI RMF Playbook (US public domain, CSV/Excel importable) and the UK Government AI Playbook (OGL v3.0). Both link-and-adapt friendly. See `content/08`. |
| **No network fallback** | If the room has no internet: run the Gandalf activity as a *thought* exercise from the printed level descriptions in `exercises/lab.md`, and lean harder on the Python parts. |

## Source & licence note

| Source | Role in this session | Reuse verdict |
|---|---|---|
| **OWASP Top 10 for LLM Applications 2025** (OWASP GenAI Security Project) | The checklist — reproduced on slides and in `content/04` | **SLIDE-SAFE** — CC BY-SA 4.0, attribute "OWASP GenAI Security Project" |
| **NIST AI RMF 1.0 + GenAI Profile (AI 600-1) + AI RMF Playbook** | Governance vocabulary; the policy-drafting exercise | **SLIDE-SAFE** — US Government work, public domain |
| **MITRE ATLAS™** | Attacker-tactic vocabulary for the injection section | **SLIDE-SAFE** with the required MITRE credit line |
| **UK Government AI Playbook** (GDS/DSIT) | Adaptable policy text for the closing exercise | **SLIDE-SAFE** — Open Government Licence v3.0 |
| **GSA Order CIO 2185.1C** | Structural model of a real signed internal AI-use policy | **SLIDE-SAFE** — US public domain |
| **"Asleep at the Keyboard?"** (Pearce et al., arXiv 2108.09293 / IEEE S&P 2022) + FormAI, Veracode | The insecure-code statistics | **Cite the figures** (facts) — do **not** reproduce paper figures |
| **Simon Willison's prompt-injection writing** incl. the three-precondition framing | Concept paraphrased and attributed | **LINK-ONLY** — all rights reserved; never reproduce prose or diagrams |
| **Lakera Gandalf** | The live interactive | **LIVE DEMO / LINK ONLY** — no screenshots on slides |
| **Anthropic / OpenAI safety and red-teaming material** | Why jailbreaking resists a full fix | **LINK-ONLY** — assign as reading |
| **"LLM System Safety and Security"** (Nield, O'Reilly) — source deck | HS/IM/TTO triangle, operating domain, Swiss cheese, systems-not-tasks | **LINK-ONLY** — paraphrase and attribute the framing |

> **Correction carried into this session (per `../../AI_input.md` §2.2 and §4).** The source deck is titled *LLM System Safety and Security* but contains **zero** adversarial-security content — no prompt injection, no jailbreaking, no OWASP mapping, no RAG attack surface, no PII taxonomy. Its "security" is system-safety in the aviation sense. We say this out loud rather than quietly papering over it: the hazard framework we borrow from it is genuinely good and we reuse it in `content/05`; **everything adversarial in this session is authored** against the public sources listed above. Do not present the source deck as covering this ground.

> **Currency warning.** Every date, version number and statistic in this session drifts. Anything marked **"verify at delivery"** in `resources/sources.md` must be re-checked before the session is taught — in particular the EU AI Act timeline (provisional), the OWASP edition, and the insecure-code percentages.
