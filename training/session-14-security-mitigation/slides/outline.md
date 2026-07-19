# Slides — Session 14: Risk II — Security, Privacy & Mitigation in Practice

Slide-by-slide spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, accessibility, licence-footer rules — not restated here). Target: **18 content slides** + title + agenda + Q&A + resources = **22 slides**, ~45 min. Speaker notes go in the Notes pane, never on the slide. Every headline is a claim, not a label.

**Licence quick-reference for this deck:**

| Status | Items |
|---|---|
| **SLIDE-SAFE (embed + attribute)** | OWASP LLM Top 10 2025 (**CC BY-SA 4.0** — attribution footer required on slides 14–15); NIST AI RMF / AI 600-1 / Playbook (US public domain); MITRE ATLAS™ (credit line required); UK Government AI Playbook (OGL v3.0); GSA CIO 2185.1C (US public domain) |
| **CITE THE FACT, don't copy the figure** | Copilot / FormAI / Veracode insecure-code statistics (slide 13) |
| **LINK-ONLY — never embed** | **Lakera Gandalf (live demo only, NO screenshots)**; Simon Willison's prose and diagrams (paraphrase + attribute the framing); Anthropic/OpenAI safety posts; vendor retention terms; the LLM-safety source deck |

**All Mermaid below is original to this course and safe to render.**

**Deck-builder warning:** every date and statistic on slides 13 and 19 is volatile. Re-check against `../resources/sources.md` before building, and again before the deck is presented.

---

## Slide 1 — Title

- **On-slide text:** "Risk II: Security, Privacy & Mitigation in Practice" · Session 14 of 16 · Block: *Use it responsibly* · AI Training Series.
- **Speaker notes:** Session 13 was about failures that come from what this technology *is*. Today is about failures that come from how you *deploy* it. One line of framing before we start, and say it plainly: this is defensive material. We look at how these systems break so we can gate them properly. Nothing here is an attack recipe.
- **Visual:** Series title layout.
- **Source/licence:** none.

## Slide 2 — Agenda

- **On-slide text:** Break a model (live) · Prompt injection · The three preconditions · Jailbreaks & guardrails · What not to paste · Generated code · The OWASP checklist · Hazard triangle & the human gate · EU AI Act · Your policy.
- **Speaker notes:** Ten beats, forty-five minutes — this is the densest session in the series and it will feel fast. Two things you take away as artefacts: the OWASP checklist and a policy draft you write yourselves. Mirror the README minute budget.
- **Visual:** Agenda layout matching the README table.
- **Source/licence:** none.

## Slide 3 — Everyone, open this link (hook)

- **On-slide text:** `gandalf.lakera.ai` · Get the password out of it. · 8 minutes. Shout out when you clear a level.
- **Speaker notes:** No setup, no signup, phones are fine. Do **not** explain the levels — the discovery is the lesson. Circulate; unstick a table if needed; keep the energy up. Have a stopwatch visible. If the network is down, run the text-only version from `exercises/lab.md`.
- **Visual:** Full-bleed URL, one line. **NO screenshots of Gandalf — proprietary.** Neutral illustration or plain type only.
- **Source/licence:** **Lakera Gandalf — LIVE DEMO / LINK ONLY. Do not embed.**

## Slide 4 — You just defeated real, engineered defences — in minutes

- **On-slide text:** L1 no defence · L2 system prompt · L3 output guard · L4 LLM classifier · L5+ input guard · Every one of them fell.
- **Speaker notes:** Run the poll: highest level reached — hands at 2,3,4,5,6,7+. Show the spread. Then the sentence that carries the session: every one of those layers was a real, deliberately-built defence, and a room of non-specialists with no tools got through several of them in eight minutes. Ask what would have made the password genuinely safe — push until someone says *not putting it in the context window*. That is the whole session.
- **Visual:** The layer stack as an ordered diagram with a broken-through arrow. Original; source table in `exercises/lab.md`.
- **Source/licence:** original diagram. Gandalf named as a live demo only.

## Slide 5 — There is no boundary between instruction and data

- **On-slide text:** Your instructions + the untrusted document = **one token stream** · The model infers authority from *pattern* · Patterns can be imitated.
- **Speaker notes:** This is the architectural fact everything today descends from. Recall Session 1: it is a pattern-matcher predicting a continuation, not a process obeying its owner. Your system prompt is one influence among several. When the untrusted text carries a stronger, more recent, more imperative pattern, that pattern can win. Slow down here — if only one slide lands today, make it this one.
- **Visual:** Original Mermaid from `content/01` §1:
  ```mermaid
  flowchart LR
    S["System prompt<br/>(your instructions)"] --> C["One concatenated<br/>token stream"]
    U["User input<br/>(untrusted)"] --> C
    D["Retrieved document / email /<br/>ticket / log (untrusted)"] --> C
    C --> M["Model:<br/>predicts the next token"]
    M --> O["Output"]
  ```
- **Source/licence:** original. Alt text: instructions and untrusted content merging into a single stream before reaching the model.

## Slide 6 — SQL injection was solved. This one is not.

- **On-slide text:** SQL: parameterisation gave the DB a **parser** — code vs. data, enforceable · LLM: no parser, no grammar, no slots · Role separation is a strong prior, **not a boundary**.
- **Speaker notes:** The beat that lands with developers. Prepared statements work because the database parses the template first and binds values into slots that can never be re-parsed as syntax. There is nothing equivalent here — for a language model, everything is language. Vendors expose system/user/tool roles and train models to weight them differently; that helps and it is not enforcement. Give the one-line version people can repeat to a colleague.
- **Visual:** Two-column comparison table from `content/01` §1, trimmed to four rows.
- **Source/licence:** original.

## Slide 7 — Direct injection is embarrassing. Indirect is the incident.

- **On-slide text:** **Direct:** the user attacks → abuse, prompt leakage, cost · **Indirect:** the *content* attacks; the attacker never touches your system · Carriers: **customer defect descriptions · OSS commit messages · build logs · vendor release notes · support email · wiki pages in a RAG index**.
- **Speaker notes:** Direct injection is mostly a content-integrity problem, and the attacker is usually already authorised — unless your app has more privilege than the user, which is the confused-deputy case worth flagging. Indirect is the one people miss, so make the carrier list local: every one of those is externally authored or externally influenceable, and every one is something this team already feeds to tooling. The wiki-index row is the worst — poison it once and it is retrieved for months, invisibly. That is why OWASP gave RAG its own 2025 entry; we get there on slide 15.
- **Visual:** Two-column layout. Left: original Mermaid from `content/01` §3. Right: the six-row carrier list.
  ```mermaid
  sequenceDiagram
    participant X as Attacker
    participant R as Content store
    participant A as Your LLM app
    participant T as Tool / API
    X->>R: Plants instructions in content
    Note over R: Weeks pass. No attack in progress.
    A->>R: Retrieves (RAG / summariser / agent)
    A->>T: Acts on the attacker's instruction
    T-->>X: Consequence lands
  ```
- **Source/licence:** original. Alt text: attacker plants text in a content store; the victim's own pipeline later executes it.

## Slide 8 — "Tell it to ignore instructions in the document" is not a control

- **On-slide text:** It **does** reduce naive success rates · It is **still just more text in the same stream** · It fails on reframing, other languages, encoding, chunk-splitting · **You can never test it to completion**.
- **Speaker notes:** Someone will propose this within thirty seconds, and it deserves a precise dismissal rather than a dismissive one — because it helps a little, which is exactly what makes it dangerous. Write it, by all means. Never let anyone cite it as the reason a design is safe. The security boundary must go somewhere a parser can enforce it.
- **Visual:** A single claim slide with the four bullets. Minimal.
- **Source/licence:** original.

## Slide 9 — Three preconditions make a deployment dangerous

- **On-slide text:** 1. Untrusted content · 2. Private data · 3. An outbound channel · **Remove any one leg → the exfiltration path breaks.**
- **Speaker notes:** The most useful design-review tool in the session. Ask the three questions in order and stop at the first *no*. If all three are yes, you do not have a prompt-engineering problem, you have an architecture problem. Flag that outbound channels are rarely labelled as such — a rendered image URL, a "share" button, a tool that fetches a link, a ticket an outsider can read. Attribute verbally: this framing is Simon Willison's; we are stating the concept in our own words.
- **Visual:** Original Mermaid from `content/01` §4:
  ```mermaid
  graph TD
    U["1. Untrusted content"] --> D["DANGEROUS<br/>combination"]
    P["2. Private data"] --> D
    E["3. Outbound channel"] --> D
    D --> R["Remove ANY ONE leg<br/>→ path breaks"]
  ```
- **Source/licence:** **concept after Simon Willison — LINK-ONLY.** Paraphrased; diagram original; no prose or figures reproduced. Footer: "Framing after Simon Willison (paraphrased)."

## Slide 10 — Wiring it to act turns a nuisance into an incident

- **On-slide text:** Text-only: bad text → a human reads it → contained · Agent: bad text → **a tool call** → executed · Loops amplify · Permissions accrete · Nobody is watching.
- **Speaker notes:** The difference between an embarrassment and an incident is whether the output *does* something. Three compounding factors with agents: a poisoned observation influences every later step of the trajectory; tokens get over-scoped because narrowing them is tedious (OWASP LLM06); and the whole point of automation is that no human is watching. Note that the source deck flagged "an API that acts directly on an LLM" years ago in a single line — it was right, and this is that line grown up.
- **Visual:** Original two-track Mermaid from `content/01` §5.
- **Source/licence:** original diagram; "API that directly acts on an LLM" framing after the LLM-safety source deck (LINK-ONLY).

## Slide 11 — Jailbreaking cannot be fully prevented — four independent reasons

- **On-slide text:** Safety is **trained**, not enforced · The input space is unbounded and **searchable** · Dual-use is genuinely fuzzy · **Guardrails are models too**.
- **Speaker notes:** Tie each reason back to the Gandalf experience. There is no `if disallowed: return` in a forward pass — there are weights that make refusal likely on inputs resembling alignment training. Attackers can search automatically. And "explain this buffer overflow" is a jailbreak from one person and a Tuesday from a Qualcomm security engineer — so some jailbreak success is the unavoidable cost of the model being useful to professionals. Closing position: providers make it harder, continuously; nobody makes it impossible, and any vendor claiming otherwise is telling you something untrue.
- **Visual:** The layered-guard Mermaid from `content/02` §3.4 (input guard → system prompt → model → output guard, with dashed evasion arrows).
- **Source/licence:** original diagram; supporting argument from Anthropic/OpenAI material — **LINK-ONLY**, resources slide only.

## Slide 12 — Four leakage paths — and one paste rule

- **On-slide text:** Transit/storage → contract · Training use → verify per **tier** · **Context → anything in the window is reachable** · Output → validation & egress · **RED** never · **AMBER** sanctioned tool only · **GREEN** free · **UNKNOWN → treat as RED**.
- **Speaker notes:** People collapse this into "does it train on my data?", which is usually the least likely path on an enterprise tier and distracts from the other three. The context path is the one that matters: secrets in a system prompt, or a RAG index that returns documents the asking user cannot open — that second one is the most common serious finding in internal deployments. Then the rule: RED is credentials, customer PII, unreleased specs, NDA material, unfixed security defects. AMBER is config baselines, defect records, logs, source — sanctioned tool only, never a personal account. Two refinements: cosmetic redaction is not de-identification (drop fields, don't mask strings), and aggregation changes the tier. Run the anonymous poll before this slide.
- **Visual:** Two-column. Left: the four-branch Mermaid from `content/03` §1. Right: the four-tier table from `content/03` §2, **populated with the team's real artefacts before delivery**. Tiers labelled in text as well as colour (greyscale-safe).
- **Source/licence:** original; OWASP LLM02/LLM08 named. ⏱ Vendor retention specifics: **do not put a number on this slide without verifying it that week.**

## Slide 13 — Compiles and works ≠ secure

- **On-slide text:** 2021 Copilot study: **~40% (39.33%)** of top suggestions in security-relevant scenarios carried a vulnerability · FormAI 2024: **~62%** · Veracode 2025: **~45%** · **Scale did not fix it.**
- **Speaker notes:** State the denominator honestly — these are security-relevant scenarios, not all code, and a technical audience will ask. State the caveat honestly too: methodologies differ, so this is **not** a clean time series and you should not draw a trend line. The defensible claim is the 40–60% band across four years and successive model generations. Then the consequence that actually matters: review, not authorship, is now the bottleneck, and SAST/SCA on generated code is the highest-return control available.
- **Visual:** The study table from `content/06` §2 (four rows) plus the feedback-loop Mermaid, or the table alone if the slide gets busy. **Do not reproduce any figure from the papers.**
- **Source/licence:** **Cite the statistics as facts** — Pearce et al. arXiv:2108.09293 / IEEE S&P 2022; FormAI; Veracode. Footer: "Figures cited; papers not reproduced." ⏱ **Verify all four numbers at delivery.**

## Slide 14 — The checklist: OWASP Top 10 for LLM Applications 2025

- **On-slide text:** LLM01 Prompt Injection · LLM02 Sensitive Information Disclosure · LLM03 Supply Chain · LLM04 Data & Model Poisoning · LLM05 Improper Output Handling · LLM06 Excessive Agency · LLM07 System Prompt Leakage · LLM08 Vector & Embedding Weaknesses · LLM09 Misinformation · LLM10 Unbounded Consumption.
- **Speaker notes:** This is the slide people photograph, and unusually for this field you can legally hand it out — CC BY-SA 4.0. Walk it fast; the depth is in `content/04`. Flag that it is a floor, not a ceiling: ten boxes ticked means ten known categories were considered, not that you are secure. The method for finding what is *not* on anybody's list is slide 16.
- **Visual:** Full table layout, ten rows, ID + name + one-line description. Ten rows breaks the ≤6-bullet rule deliberately — build it as a **table slide**, not a bullet slide, per `powerpoint_instructions.md` §4.
- **Source/licence:** **OWASP Top 10 for LLM Applications 2025 — OWASP GenAI Security Project. CC BY-SA 4.0. SLIDE-SAFE.** Attribution footer **required**. ShareAlike: fine internally; any external derivative must carry CC BY-SA 4.0.

## Slide 15 — The two new entries tell you what changed in *deployment*

- **On-slide text:** **LLM07 System Prompt Leakage** → treat the system prompt as **public** · **LLM08 Vector & Embedding Weaknesses** → your RAG index is an attack surface · Agents became normal. RAG became normal.
- **Speaker notes:** LLM07's real point is not that someone read your prompt — it is that teams put things in prompts that were acting as security controls: keys, thresholds, internal system names. LLM08 covers four distinct failures: index poisoning, access-control bypass at retrieval, cross-tenant bleed, and embedding inversion. The line that surprises people: **an embedding is not anonymisation** — if the corpus is confidential, the vector store is confidential.
- **Visual:** The LLM08 four-failure-mode table from `content/04` §2.
- **Source/licence:** **OWASP LLM Top 10 2025, CC BY-SA 4.0.** Attribution footer required.

## Slide 16 — Shrink the triangle, or collapse it

- **On-slide text:** **HS** — wrong information · **IM** — what turns it into harm · **TTO** — who is hurt, how badly · Reduce one → risk falls · **Eliminate one → the hazard is gone.**
- **Speaker notes:** The method, borrowed from aviation and automotive system safety. The key abstraction for LLMs: the hazard source is always the same thing — a piece of wrong information — whether it arrived by hallucination, injection, jailbreak, or a poisoned index. That is why one method covers all of today. Two rules, and the second is the powerful one: eliminating any component collapses the triangle entirely.
- **Visual:** Original hazard-triangle Mermaid from `content/05` §1:
  ```mermaid
  graph TD
    HS["HS — Hazard Source<br/>wrong / injected information"]
    IM["IM — Initiating Mechanism<br/>what turns it into harm"]
    TTO["TTO — Target / Threat Outcome<br/>who is hurt, how badly"]
    HS --- IM
    IM --- TTO
    TTO --- HS
  ```
- **Source/licence:** framing after the LLM-safety source deck — **LINK-ONLY**, paraphrased. Diagram original. Footer: "Framing after Nield, *LLM System Safety and Security* (paraphrased)."

## Slide 17 — Worked: the automated release-notes agent

- **On-slide text:** HS: injected or hallucinated draft · IM: **the scheduled publish** · TTO: customer acts on false info; irreversible · Remove the auto-publish → **triangle collapses**.
- **Speaker notes:** Run poll 3 before revealing the analysis: safe / unsafe / it depends. The room will split, and the split is the lesson — those voting safe are thinking about drafting (fine), those voting unsafe are thinking about publishing (the initiating mechanism). "It depends" is right, and what it depends on is exactly one design decision. Same model, same data, same prompt; one architectural change moves it from unsafe to safe.
- **Visual:** Before/after pair: the triangle with the IM present, then with it removed. Original.
- **Source/licence:** original worked example; hazard framing after the source deck (LINK-ONLY).

## Slide 18 — Constrain it to do less — and gate every action

- **On-slide text:** Operating domain: **what data · what tasks · how the user verifies** · Write the **barred** uses, not just the allowed ones · **No automated pipeline acts on model output without a qualified human gate.**
- **Speaker notes:** The gate rule is the one instruction from today that changes behaviour, so land all three words. *Automated* — unattended action is the danger, drafting is fine. *Qualified* — Session 13 said human-in-the-loop is necessary but not sufficient; a reviewer who cannot catch the error is a rubber stamp that also creates false confidence. *Gate* — it must be able to stop the action, and stopping is the default. Then the inversion: if the model is right 99% of the time, catching the 1% gets **harder**, not easier — vigilance decays, so a better model needs *more* discipline at the gate, not less.
- **Visual:** Original operating-domain chain Mermaid from `content/05` §2:
  ```mermaid
  flowchart LR
    D["1. What DATA<br/>goes in"] --> L["LLM"]
    L --> U["2. What TASKS<br/>are sanctioned"]
    U --> V["3. How the USER<br/>verifies"]
    V --> A["Real-world actions"]
  ```
- **Source/licence:** framing after the LLM-safety source deck — **LINK-ONLY**, paraphrased. Diagram original.

## Slide 19 — EU AI Act: you are a deployer, and three things apply

- **On-slide text:** **In force now:** Art. 5 prohibited practices (incl. workplace emotion recognition) · Art. 4 **AI literacy — this training counts** · **From 2026-08-02:** Art. 50 transparency · **High-risk deployer duties: reportedly deferred to 2027-12-02 — PROVISIONAL.**
- **Speaker notes:** Sixty seconds if you are running long, and you will be. Most of the Act targets *providers*; you are a deployer, which limits obligations sharply. Say the provisional caveat out loud — a training session that asserts an unconfirmed date gets quoted back as fact in a compliance conversation. Note the pleasing convergence: the high-risk duties (human oversight, logging, monitoring) are exactly what slide 18 already told you to do. Not legal advice; legal and compliance own the real position.
- **Visual:** The timeline table from `content/07` §2, with the 2027-12-02 row marked **PROVISIONAL** in text (not by colour alone).
- **Source/licence:** European Commission / AI Act implementation timeline — official EU material, paraphrased. ⏱ **RE-VERIFY EVERY DATE BEFORE DELIVERY.** Footer: "Provisional — verify at delivery."

## Slide 20 — Your homework: write the policy yourselves

- **On-slide text:** 9 sections · §4 data tiers · §5 sanctioned & barred uses · §6 the gate rule · §8 incident reporting · Built from **NIST AI RMF Playbook** (public domain) + **UK AI Playbook** (OGL v3.0).
- **Speaker notes:** Four groups, one section each, in the Q&A block or as follow-up. The document is a by-product; the *conversation* is the deliverable, because filling in "what may not be pasted" forces the classification argument. Two warnings: a policy that lists prohibitions without an approval path gets ignored, and one with no incident section produces an organisation that believes it has had no incidents. Both source playbooks are licence-clean — no procurement conversation needed.
- **Visual:** The nine-section Mermaid from `content/08` §3.
- **Source/licence:** **NIST AI RMF Playbook — US public domain. UK Government AI Playbook — OGL v3.0 (attribution required). GSA CIO 2185.1C — US public domain. All SLIDE-SAFE.**

## Slide 21 — Q&A / discussion

- **On-slide text:** "Where in your area does an automated system already act on output that nobody reads?" · Poll results recap · Monday commitments.
- **Speaker notes:** Run the seed question from `exercises/discussion.md`. The best answers are usually not about AI at all — auto-closed defects, scheduled customer reports, config sync jobs — and the realisation is that the human gate was already missing in places, and adding an LLM there is what makes it dangerous. Close by going round for one concrete Monday change each; push vague answers once for specificity.
- **Visual:** Discussion/poll layout.
- **Source/licence:** none.

## Slide 22 — Resources & credits

- **On-slide text:** OWASP LLM Top 10 2025 (CC BY-SA 4.0) · NIST AI RMF & GenAI Profile (public domain) · MITRE ATLAS™ (© MITRE, reproduced with permission) · UK AI Playbook (OGL v3.0) · GSA CIO 2185.1C · Pre-read: Simon Willison on prompt injection · Live demo: gandalf.lakera.ai · Full list: `resources/sources.md`.
- **Speaker notes:** Point at the pre-read — Willison's posts are twenty minutes and the single best preparation for this session. Note which items were embedded (the CC/public-domain ones, attributed in footers) and which were only linked. Licences and verification dates live in `resources/sources.md`.
- **Visual:** Resources & credits layout with full licence attributions, including the MITRE credit line verbatim.
- **Source/licence:** as listed.

---

### Slide-to-content map

| Slides | Content file |
|---|---|
| 3–4 | `exercises/lab.md` Part 1 |
| 5–10 | `content/01-prompt-injection.md` |
| 11 | `content/02-jailbreaking.md` |
| 12 | `content/03-data-leakage-and-privacy.md` |
| 13 | `content/06-insecure-code-generation.md` |
| 14–15 | `content/04-owasp-llm-top-10-2025.md` |
| 16–18 | `content/05-hazard-triangle-and-operating-domain.md` |
| 19 | `content/07-eu-ai-act-for-deployers.md` |
| 20 | `content/08-writing-your-ai-use-policy.md` |

---

### Build checklist (this deck)

- [ ] 18 content slides (3–20) + title, agenda, Q&A, resources = **22 slides**.
- [ ] Slides 14 and 15 carry the **OWASP CC BY-SA 4.0 attribution footer**. Slide 14 is built as a **table layout**, not bullets.
- [ ] Slide 9 footer reads "Framing after Simon Willison (paraphrased)" and reproduces **none** of his text or diagrams.
- [ ] Slides 16, 17, 18 footer-credit the LLM-safety source deck as a *paraphrased framing*; no source-deck art reproduced.
- [ ] **No Gandalf screenshots anywhere.** Slide 3 is a URL and live demo only; slide 4 uses an original diagram.
- [ ] Slide 13 reproduces **no figures** from the cited papers; all four statistics re-verified against `resources/sources.md`.
- [ ] Slide 19 shows "PROVISIONAL" **as text**, not by colour, and every date re-verified this week.
- [ ] Slide 12 tiers are labelled in text as well as colour (greyscale- and colour-blind-safe) and populated with the team's real artefacts.
- [ ] Alt text on every diagram; 18 pt minimum; ≥4.5:1 contrast; readable in greyscale.
- [ ] Speaker notes present on every content slide.
- [ ] Rehearsed at ~45 min. Slides 3–5 and 18 get the most air; **slide 19 is the first thing cut**, then slide 11, then slide 14 reduced to "the four that apply to us."
- [ ] Text-only Gandalf fallback prepared in case the room has no network.
