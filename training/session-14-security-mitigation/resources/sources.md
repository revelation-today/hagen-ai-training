# Sources — Session 14

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / CC-BY-SA / public-domain / standards body / explicit royalty-free grant. May derive slide and content text and figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NC / ND / vendor / proprietary game / internal. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY.

> **This session carries more authored material than any other in the series.** The source deck named in #8 has the word "Security" in its title and contains **zero** adversarial-security content (see `../../AI_input.md` §2.2). Its hazard framework is genuinely good and is reused in `content/05`; everything in `content/01`–`04` and `06`–`08` is authored against the public sources below.

> **Currency.** Items marked ⏱ **verify at delivery** are volatile: versions, dates, statistics, and vendor terms all drift. The EU AI Act timeline (#12) is the most volatile item in the session and contains one **provisional** entry.

---

## Slide-safe (embeddable with attribution)

**#1 — OWASP Top 10 for LLM Applications 2025** — OWASP GenAI Security Project (OWASP Foundation).
- Landing: `https://genai.owasp.org/llm-top-10/`
- PDF: `https://owasp.org/www-project-top-10-for-large-language-model-applications/assets/PDF/OWASP-Top-10-for-LLMs-v2025.pdf`
- Version/date: **v2025, published 2024-11-17** (PDF build tag v4.2.0a, 2024-11-14). Supersedes v1.0/v1.1.
- Licence: **CC BY-SA 4.0** → **SLIDE-SAFE.**
- Required attribution wherever reproduced: *"OWASP Top 10 for LLM Applications 2025 — OWASP GenAI Security Project. Licensed CC BY-SA 4.0."*
- **ShareAlike caveat:** internal Qualcomm use is unproblematic. Any derivative distributed **externally** must itself carry CC BY-SA 4.0.
- Used for: the whole of `content/04`; LLM01 in `content/01`; LLM02/LLM08 in `content/03`; LLM03/LLM05 in `content/06`; the review-gate table in `content/04` §3 (structure derived, "where it shows up for this team" column authored).
- ⏱ **Verify at delivery:** confirm 2025 is still the current numbered edition.

**#2 — NIST AI Risk Management Framework 1.0, the Generative AI Profile (NIST AI 600-1), and the AI RMF Playbook** — National Institute of Standards and Technology (US).
- AI RMF: `https://www.nist.gov/itl/ai-risk-management-framework`
- GenAI Profile PDF: `https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf`
- Playbook: `https://airc.nist.gov/airmf-resources/playbook/`
- Dates: AI RMF 1.0 (Jan 2023); **AI 600-1 published 2024-07-26.**
- Licence: **US Government work — public domain (17 U.S.C. §105).** **SLIDE-SAFE, unrestricted.** Courtesy attribution to NIST recommended.
- Used for: Govern/Map/Measure/Manage vocabulary and the policy structure in `content/08`; Data Privacy and Information Security risk categories in `content/03`.
- Note: the Playbook downloads as **PDF, CSV, Excel and JSON** — the sub-categories import directly into a control spreadsheet, which is what makes it the best template source for `content/08`.

**#3 — MITRE ATLAS™** (Adversarial Threat Landscape for AI Systems) — The MITRE Corporation.
- `https://atlas.mitre.org/` · Navigator: `https://mitre-atlas.github.io/atlas-navigator/`
- Version: ~v5.x living knowledge base; ~16 tactics, 80+ techniques, 40+ case studies. 2025 releases added GenAI and agent techniques (incl. RAG poisoning, LLM prompt crafting).
- Licence: **MITRE royalty-free terms — reuse permitted with attribution.** **SLIDE-SAFE with the credit line.**
- **Required credit line:** *"© 2026 The MITRE Corporation. This work is reproduced and distributed with the permission of The MITRE Corporation."* First written mention: "MITRE ATLAS™".
- Used for: attacker-tactic vocabulary in `content/01` and the jailbreak taxonomy framing in `content/02` §2; named as the threat-modelling framework in `content/04` §4.
- ⏱ **Verify at delivery:** update the year in the credit line.

**#4 — Open-source red-team harnesses** (documentation is slide-safe; also usable as presenter demos).

| Tool | Org | URL | Licence | Version at research date |
|---|---|---|---|---|
| **promptfoo** | promptfoo | `https://www.promptfoo.dev/docs/red-team/` · `https://github.com/promptfoo/promptfoo` | **MIT** | latest via `npx promptfoo@latest` |
| **garak** | NVIDIA | `https://github.com/NVIDIA/garak` · `https://garak.ai/` | **Apache-2.0** | **v0.15.1 (2026-06-05)** ⏱ |
| **PyRIT** | Microsoft AI Red Team | `https://github.com/microsoft/PyRIT` | **MIT** | active |

- **SLIDE-SAFE** — screenshots and quickstarts may be reproduced with attribution.
- Used for: `content/02` §4 (adversarial scan as a release-gate artefact). promptfoo ships OWASP LLM Top 10 / NIST AI RMF / MITRE ATLAS presets, which is why it is named first.
- Not used as an audience hands-on — setup cost is too high for a 45-minute session. Presenter demo or pre-recorded only.

**#14 — UK Government AI Playbook** — UK Government Digital Service / DSIT, Feb 2025.
- `https://www.gov.uk/` (search "AI Playbook for the UK Government" — HTML edition)
- Licence: **Open Government Licence v3.0** — copy, adapt, and use commercially **with attribution**. **SLIDE-SAFE.**
- Used for: adaptable principles text in `content/08` §2. Aimed at organisations *using* AI, which is exactly the deployer posture of this session.

**#15 — GSA Order CIO 2185.1C** — US General Services Administration, Mar 2026.
- GSA directives library, `https://www.gsa.gov/`
- Licence: **US Government work — public domain.** **SLIDE-SAFE.**
- Used for: the structural model of a real signed internal AI-use policy in `content/08` §3 — applicability scope, roles, prohibited-input rules, GenAI output labelling. Strip the US-statutory scaffolding when adapting.

---

## Cite-the-fact (statistics are facts; do not reproduce the figures or tables)

**#5 — Insecure code generation.**

- **Pearce, Ahmad, Tan, Dolan-Gavitt & Karri (NYU), *Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions*.** arXiv:**2108.09293**; **IEEE S&P (Oakland) 2022**; also CACM 2025 (doi 10.1145/3610721). `https://arxiv.org/abs/2108.09293`
  - **The figure:** 89 scenarios → **1,689 generated programs, ~40% vulnerable.** The commonly quoted precise value is **39.33%**; the paper's abstract rounds to "approximately 40%." **Report as "~40% (39.33%)" and ⏱ verify the exact value and denominator against the paper before slideing it** — a technical audience will check, and the denominator (security-relevant scenarios, not all code) matters.
  - Licence: arXiv non-exclusive licence; author copyright. **The statistic is a citable fact. Do NOT reproduce the paper's figures or tables.**
- **FormAI benchmark** — 51% of GPT-3.5 outputs vulnerable; 2024 replication across nine models → **~62%**.
- **Veracode / industry evaluation (2025)** — **~45%** of generated code introduced a security flaw across many models.
- ***Hidden Risks of LLM-Generated Web Application Code*** — arXiv:**2504.20612** (2025-04-29).
- Used for: the whole of `content/06`. ⏱ **All figures verify at delivery** — this is the fastest-moving table in the session. Note in the room that methodologies differ, so these are **not** a clean time series; the defensible claim is "the 40–60% band, and scale did not solve it."

---

## Link-only (reference / assign / live-demo — never embed)

**#6 — Lakera Gandalf** — `https://gandalf.lakera.ai/`
- Licence: proprietary game (Lakera AI). **LIVE DEMO / LINK ONLY. No screenshots on slides.**
- Used for: the session hook and `exercises/lab.md` Part 1. Zero setup, browser only, no signup.
- ⏱ **Verify at delivery:** confirm the site is up and the level structure is unchanged; complete levels 1–4 yourself beforehand. Have the text-only fallback in `exercises/lab.md` ready.
- Alternatives if Gandalf is unavailable: **Wiz Prompt Airlines** (`https://promptairlines.com/`) and **PromptTrace** (`https://prompttrace.airedlab.com/`) — also zero-setup and browser-based; PromptTrace additionally shows the assembled prompt stack and has a *defenses* module. **HackAPrompt** (`https://www.hackaprompt.com/`) is more curriculum-like but requires an account. All proprietary — **live/link only**.

**#7 — Simon Willison — prompt injection and the three-precondition ("lethal trifecta") framing.**
- Key post: *The lethal trifecta for AI agents: private data, untrusted content, and external communication* — `https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/` (2025-06-16). He also coined the term "prompt injection" in 2022.
- Licence: blog footer reads **© 2002–2026, all rights reserved — no Creative Commons licence.** → **LINK-ONLY.**
- **How this session uses it:** the *concept* is paraphrased in `content/01` §4 and attributed as "framing after Simon Willison." Ideas are not copyrightable; his expression is. **Do not paste his prose or diagrams onto a slide.** Quote at most a sentence, with attribution, if you must.
- The canonical and most-cited voice on this topic. **Assign the post as pre-reading** — it is the single best thing a participant can read before the session.

**#8 — Nield, T., *LLM System Safety and Security* (O'Reilly live training).** Source deck.
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- Used for: the **HS/IM/TTO hazard triangle**, the **operating domain** prescription, the **Swiss cheese** application, **"you make a system safer by constraining it to do less,"** **"think in systems, not tasks,"** the **99%/1% human-factor observation**, the **"an API that directly acts on an LLM"** initiating mechanism, the **"compiles and works ≠ secure"** phrasing, and the A/B/C case-poll format in `exercises/discussion.md`.
- All of the above are **paraphrased and re-expressed**; every diagram in `content/05` is original to this course. Attribute the *framings* verbally; do not reproduce the deck's slides.
- **Documented gap carried into this session:** the deck contains no prompt injection, no jailbreaking, no OWASP mapping, no RAG attack surface, no PII taxonomy, and delivers its stated "organisational policy" objective as five exhortations rather than a template (`../../AI_input.md` §2.2, §4 gap #6). `content/08` fills that gap.

**#9 — Anthropic — safety and red-teaming material.**
- Responsible Scaling Policy v3.0 (2026-02-24): `https://www.anthropic.com/news/responsible-scaling-policy-v3`; ASL-3 safeguards activated May 2025 (Claude Opus 4); Frontier Safety / Risk Reports.
- Licence: © Anthropic. **LINK-ONLY** — vendor material; say so in the room.
- Used for: background to `content/02` §3 on why jailbreaking resists a full fix. Assign as reading.

**#10 — OpenAI — external red-teaming methodology and system cards.**
- *OpenAI's Approach to External Red Teaming for AI Models and Systems* — arXiv:**2503.16431** (2025): `https://arxiv.org/abs/2503.16431`. System Cards + Preparedness Framework: `https://openai.com/index/strengthening-safety-with-external-testing/`
- Licence: © OpenAI / author copyright. **LINK-ONLY** — cite the methodology, do not reproduce.
- Used for: background to `content/02` §4. The arXiv paper is the best single red-teaming-methodology reference to assign.

**#11 — Vendor data-retention and training-use terms** (OpenAI, Anthropic, Google, Microsoft, and any internal deployment).
- Licence: vendor documentation, all rights reserved. **LINK-ONLY.**
- Used for: `content/03` §1. ⏱ **These terms change and differ per product and per tier. Verify per product at delivery and record the answer in the policy artefact from `content/08` — do not assert a specific retention period on a slide without checking it that week.** The consumer-vs-enterprise tier distinction is where most accidental exposure happens.

**#12 — EU AI Act — official material and implementation timeline.**
- `https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai` · `https://artificialintelligenceact.eu/implementation-timeline/`
- Licence: official EU material — reference and paraphrase. Treat as **LINK-ONLY** for prose; the dates are facts.
- Used for: the whole of `content/07`.
- ⏱ **MOST VOLATILE ITEM IN THIS SESSION.** Settled: Art. 5 prohibited practices and Art. 4 AI literacy in force **2025-02-02**; Art. 50 transparency applies **2026-08-02**; GPAI enforcement powers **2026-08-02**; full application (Annex I) **2027-08-02**. **PROVISIONAL:** high-risk (Annex III) **deployer** obligations reportedly re-anchored from 2026-08-02 to **2027-12-02** by a **2026-05-07 provisional political agreement ("Digital Omnibus")**. **Confirm final adopted text before relying on it, and mark it as provisional on the slide.** This session is training material, not legal advice.

**#13 — EU AI Act Service Desk — Article 4 (AI literacy).** — EU AI Office.
- `https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-4`
- Official EU reference for the AI-literacy deployer duty. Used in `content/07` §3 as the basis for "this training series is the artefact."

**#16 — NSW AI Assessment Framework** — Digital NSW (Feb 2026). `https://www.digital.nsw.gov.au/`
- A working **Excel** risk-triage self-assessment across the AI lifecycle.
- Licence: Copyright NSW, no CC stated. **ADAPT STRUCTURALLY / verify terms before reuse.** Referenced in `content/08` §2.

**#17 — CNIL AI self-assessment guide** — Commission Nationale de l'Informatique et des Libertés (French DPA).
- `https://www.cnil.fr/en/self-assessment-guide-artificial-intelligence-ai-systems`
- Analysis grid plus seven fact sheets for GDPR/AI maturity self-assessment.
- Licence: French public body, no explicit CC notice. **REFERENCE / verify before reuse.** Referenced in `content/08` §2.

> **Unverified — do not assert their contents.** The Australian DTA AI policy and the Singapore IMDA/PDPC AI governance framework timed out during research; the ICO session-4 PDF returned 403. SANS had no AI-specific policy template at fetch time. Check manually if you want to cite any of them.

---

## Further reading (LINK-ONLY, high quality — assign, don't slide)

| Topic | Suggestion | Why |
|---|---|---|
| **The best single pre-read for this session** | Simon Willison's prompt-injection posts, especially the three-precondition one (#7) | The canonical voice. Twenty minutes, and the room arrives already understanding `content/01` |
| Red-teaming as a discipline | OpenAI, arXiv:2503.16431 (#10) | A concrete, teachable methodology — phased testing, 100+ red-teamers, 45 languages |
| Why alignment does not close the jailbreak gap | Anthropic's Responsible Scaling Policy and Frontier Safety reports (#9) | Vendor material, and worth reading *as* vendor material — note what it claims and what it carefully does not |
| Governance at organisation scale | NIST AI 600-1 GenAI Profile (#2) | Twelve GenAI risk categories with 200+ suggested actions. Public domain, so you can lift it wholesale |
| Attacker's-eye view | MITRE ATLAS™ case studies (#3) | Real incidents in ATT&CK-style form — the vocabulary security teams already speak |
| The checklist itself, in full | OWASP LLM Top 10 2025 PDF (#1) | Each entry has scenarios and mitigations well beyond the summary table in `content/04` |

---

### Corrections and gaps carried into this session

| Issue | Where addressed |
|---|---|
| Source deck titled "…Safety and **Security**" contains no adversarial security content (`../../AI_input.md` §2.2) | Stated plainly in `README.md` and in this file. All adversarial content authored |
| Source deck's policy objective delivered as exhortation, not a template (`../../AI_input.md` §4 gap #6) | `content/08` supplies a nine-section structure plus reuse-safe template sources |
| Source deck never mentions the EU AI Act (`../../AI_input.md` §5) | `content/07`, with the provisional-date caveat made explicit |
| Source deck's Copilot statistic is accurate but four years old and never re-checked | `content/06` §2 adds the 2023–2025 replications and the honest "not a clean time series" caveat |
| Source deck's "an API that directly acts on an LLM" is one undeveloped line | Developed across `content/01` §4–5 and `content/05` §4 into the human-gate rule |
