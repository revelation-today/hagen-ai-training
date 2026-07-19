# Slides — Session 13: Risk I — When AI Is Confidently Wrong

Slide-by-slide spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, accessibility, licence-footer rules — not restated here). Target: **16 content slides** + title + agenda + Q&A + resources = **20 slides**, ~45 min. Speaker notes go in the Notes pane, never on the slide. Every headline is a claim, not a label.

**Licence quick-reference for this deck**

- **SLIDE-SAFE (embed + attribute):** scikit-learn (BSD-3) — code, metric formulas, `classification_report` output. Maynez et al. 2020 (CC BY 4.0) — intrinsic/extrinsic definitions. RAGAS / ARES via ACL Anthology (CC BY 4.0) — groundedness-evaluation framing. Ioannidis 2005 (PLoS, CC BY — verify at delivery) — cite rather than quote. **All statistics, Bayes arithmetic, tables and Mermaid below are original to this course** and safe to render.
- **LINK-ONLY (never embed; paraphrase / resources slide only):** the DL Day 3 and LLM-Safety source decks; the Coase, Ng and Chollet quotations. Paraphrase the ideas; attribute the framing verbally; put no quotation marks on a slide.

**The one build note that matters.** Slides 10–14 are the vendor reveal. They must be built as a **progressive reveal — one number per click, nothing pre-visible**. If the room can read "3.39%" while the presenter is still saying "99%", the session's best asset is dead. If the deck tool cannot animate, use five separate slides (as specced) and never a summary slide before slide 14.

---

## Slide 1 — Title

- **On-slide text:** "Risk I: When AI Is Confidently Wrong" · Session 13 of 15 · Block: *Risk it* · AI Training Series.
- **Speaker notes:** This session is about being fooled by a true number. Three parts: hallucination and what actually mitigates it; why your metric lies; and a vendor role-play that ends in a buy/don't-buy decision. Nothing today requires code. The last part is the one you will use within the month.
- **Visual:** Series title layout.
- **Source/licence:** none.

## Slide 2 — Agenda

- **On-slide text:** Hallucination + mitigations · The 99% trap · Why your metric lies · Base rates · The vendor role-play · P-hacking · Q&A.
- **Speaker notes:** Mirror the README minute-budget. Flag that the vendor role-play at minute 28 is the centrepiece and that we are deliberately leaving the hook unresolved until then.
- **Visual:** Agenda layout matching the README table.
- **Source/licence:** none.

## Slide 3 — Would you buy this? (hook — do not resolve)

- **On-slide text:** *"Our AI identifies **99%** of at-risk cases."* · Buy it? · Don't buy it? · Need more information?
- **Speaker notes:** Poll the room by show of hands and **write the counts on a flipchart** — you will return to them at minute 39 and the change is the emotional payoff. Take one "need more info" answer and ask what they'd ask for; do not evaluate it. Say explicitly: "we come back to this at minute 28." Then move on. Resisting the urge to resolve this now is what makes the reveal work.
- **Visual:** Full-bleed quote card, three poll options. Deliberately sparse.
- **Source/licence:** original.

## Slide 4 — Fluent and wrong leave by the same door

- **On-slide text:** Dense data → interpolation → usually right · Sparse data → extrapolation → possibly invented · **Same fluency. Same confidence. No warning label.**
- **Speaker notes:** Session 1 recap in 90 seconds. The model cannot flag its own extrapolation, because fluency is produced by the same machinery either way. So "it sounded confident" carries exactly zero information about whether it is true. That single fact is why every mitigation in the next four slides is about *external* checking.
- **Visual:** Mermaid from `content/01` §1:
  ```mermaid
  flowchart LR
    P["Prompt"] --> R{"Densely covered<br/>pattern space?"}
    R -->|"Dense — interpolation"| G["Fluent output<br/>usually correct"]
    R -->|"Sparse — extrapolation"| H["Fluent output<br/>possibly invented"]
    G --> S["Identical confidence,<br/>no warning label"]
    H --> S
  ```
- **Source/licence:** original diagram. Alt text: two paths through a model that converge on identically confident output.

## Slide 5 — Two kinds of hallucination, two different fixes

- **On-slide text:** **Intrinsic** — contradicts the source you gave it · **Extrinsic** — unverifiable from that source · Grounding turns *extrinsic* into *intrinsic* · Intrinsic is checkable. That is the win.
- **Speaker notes:** Definitions after Maynez et al. 2020 — put the attribution in the footer. Why a manager cares: the two failure modes have different mitigations, and buying the wrong one is common. Intrinsic errors can be checked against a document you possess, which means they can be automated. Extrinsic errors need an outside source of truth. Grounding moves work from the second bucket to the first — real progress, not a cure.
- **Visual:** The two-column comparison table from `content/01` §2, trimmed to definition + example + detectability.
- **Source/licence:** **Maynez et al. 2020, ACL Anthology, CC BY 4.0** — footer attribution required.

## Slide 6 — RAG makes hallucination auditable, not absent

- **On-slide text:** Retrieve → answer only from retrieved text → cite → **check every claim against its citation** · New failures: retrieval miss · wrong-but-similar passage · synthesis across two correct citations · stale index.
- **Speaker notes:** Walk the pipeline, then land on the failures, because the failures are what nobody says in a vendor demo. The dangerous one is the second: a real citation attached to an inapplicable passage — a different release, a superseded policy. The citation being *right there* is what stops the reviewer looking. Then the good news: groundedness is measurable — RAGAS, ARES — so "is this answer faithful to its source?" has published methodology and a number. If a vendor can't give you a faithfulness number, they haven't measured the thing you care about.
- **Visual:** Mermaid from `content/01` §3 (the RAG loop with the post-answer check node emphasised).
- **Source/licence:** original diagram. RAGAS (Es et al., EACL 2024) and ARES (Saad-Falcon et al., NAACL 2024) named in footer — **ACL Anthology, CC BY 4.0**.

## Slide 7 — The verification paradox: cheap checking is a property of *bad* models

- **On-slide text:** Weak model → errors are loud → verify in seconds · Excellent model → errors are indistinguishable → verify by re-deriving · **The better it gets, the more verification costs.**
- **Speaker notes:** This is the counter-intuitive turn and the bridge to the next slide. Most AI business cases assume verification is free. If the model is *good*, verification costs the full price of doing the task — and if the team skips it to preserve the saving, you have automated the appearance of the work while deleting the check. Ask the room: whose business case survives if you price verification honestly?
- **Visual:** The three-row table from `content/01` §4 (model quality / errors per 100 / what an error looks like / cost to verify).
- **Source/licence:** original. Framing after the LLM-Safety source deck (LINK-ONLY) — attribute verbally, embed nothing.

## Slide 8 — If it's right 99% of the time, spotting the 1% is *harder*

- **On-slide text:** Expectation shifts to "usually right" · Attention thins as throughput grows · **The surviving errors are the subtlest ones — the obvious ones were fixed first** · Model improvement ≠ risk reduction.
- **Speaker notes:** The headline sentence of the session for this audience. Three mechanisms; the third is the one people miss and the strongest — improvement is *selective*, so the residual 1% is not a random sample of the old 30%, it is the hardest-to-detect part of it. Automation complacency / startle factor is documented human-performance science, not a discipline problem; aviation designed around it decades ago. Consequence for this room: **a model upgrade invalidates your risk assessment.** That's a change-control instinct and it applies directly.
- **Visual:** Mermaid from `content/02` §2:
  ```mermaid
  flowchart TD
    A["Model accuracy rises<br/>99% correct"] --> B["Reviewer sees a long<br/>run of correct outputs"]
    B --> C1["Expectation shifts:<br/>'this is usually right'"]
    B --> C2["Review time per item<br/>falls to near zero"]
    B --> C3["Errors become subtle —<br/>the easy ones were fixed first"]
    C1 --> D["Detection probability<br/>collapses"]
    C2 --> D
    C3 --> D
  ```
- **Source/licence:** original diagram. Framing after the LLM-Safety source deck (LINK-ONLY). Alt text: rising accuracy feeding three mechanisms that all reduce human detection.

## Slide 9 — "There's a human in the loop" is not a control

- **On-slide text:** Qualified in the subject matter? · Source of truth at review time? · Time per item × items per day — do the multiplication · Measured on **catch rate** or throughput? · **Could they have produced the right answer themselves?**
- **Speaker notes:** Necessary and not sufficient. Run the five questions; any "no" makes the gate theatre. The last line is the fitness test and the sharpest one: if the reviewer couldn't derive the answer independently, they are rating *writing style* — and writing style is precisely where a good model never fails. Then the three controls that survive an improving model: sample and independently re-derive; seed known-bad items and measure the catch rate; treat a model version as a configuration item.
- **Visual:** The decision Mermaid from `content/02` §4:
  ```mermaid
  flowchart TD
    H["'We have a human in the loop'"] --> Q1{"Qualified in the<br/>subject matter?"}
    Q1 -->|No| T["Theatre"]
    Q1 -->|Yes| Q2{"Source of truth<br/>available at review?"}
    Q2 -->|No| T
    Q2 -->|Yes| Q3{"Time per item ×<br/>items per day fits<br/>a working day?"}
    Q3 -->|No| T
    Q3 -->|Yes| Q4{"Measured on catch rate,<br/>not throughput?"}
    Q4 -->|No| T
    Q4 -->|Yes| C["A real control.<br/>Now sample it."]
  ```
- **Source/licence:** original. Alt text: four sequential gates; any "no" leads to the node labelled Theatre.

## Slide 10 — 98% accurate. Zero useful.

- **On-slide text:** Model rule: *"If they're called Michael, they'll quit."* · 100 employees · The one Michael **stayed** · Someone else **quit** · Wrong on both cases that matter · **Accuracy: 98%.**
- **Speaker notes:** Tell it as a story, land the punchline, pause. Then twist the knife: why stop at Michael — predict *nobody* ever quits and score **99%**, with no model, no data, no features. Machine learning takes shortcuts exactly like this, and it does so precisely when the event is rare: attrition, fraud, security breaches, escaping defects, production incidents. The rarer and more valuable the event, the higher the accuracy a useless model reports.
- **Visual:** Simple narrative graphic — 100 person icons, one flagged, one departing, neither the same. No chart.
- **Source/licence:** original retelling. Parable structure from the DL Day 3 source deck — **LINK-ONLY**, reproduce nothing.

## Slide 11 — The four-cell table that accuracy hides

- **On-slide text:** The confusion matrix (rows = truth, columns = prediction): TP 0 · FN 1 · FP 1 · TN 98 · Sensitivity **0** · Precision **0** · F1 **undefined** · Accuracy **.98**.
- **Speaker notes:** State the row/column convention out loud before reading anything — half of all confusion-matrix confusion is convention confusion. Read the top row: of the one person who quit, we found zero. Read the left column: of the one we flagged, zero were right. Both numbers you care about are zero. **F1 is not low, it is mathematically undefined** — 0/0 — and an undefined F1 is a screaming alarm. Note that scikit-learn renders it as 0.00, which many dashboards inherit and hide. Mention we corrected a cell-label transposition in the source deck; the values were unaffected only by luck.
- **Visual:** Two side-by-side tables from `content/03` §2–3: the 2×2 matrix, and the six metrics with formula + value. Colour-code TP/TN vs. FP/FN **with shape or label as well as colour** (greyscale-safe).
- **Source/licence:** metric definitions and formulas per **scikit-learn (BSD-3)** — footer attribution.

## Slide 12 — Sensitivity and precision point in opposite directions

- **On-slide text:** **Sensitivity** = P(flagged | truly positive) — *what a vendor quotes* · **Precision** = P(truly positive | flagged) — *what you live with* · Flag everyone → sensitivity 100% · **The gap between them depends on how rare the positive class is.**
- **Speaker notes:** This slide is the hinge of the whole deck — everything after it is this one idea with money attached. Sensitivity is measured only on the positive cases, so it is blind to how many negatives exist, and it can always be driven to 1.0 by flagging everything. Which means sensitivity alone is never evidence. Say the last bullet slowly; it is the setup for slide 14.
- **Visual:** The two-subgraph Mermaid from `content/03` §3:
  ```mermaid
  flowchart LR
    subgraph SENS["Sensitivity — start from the TRUTH"]
      A["Of everyone who<br/>really is positive…"] --> B["…how many did<br/>we flag?"]
    end
    subgraph PREC["Precision — start from the PREDICTION"]
      C["Of everyone we<br/>flagged…"] --> D["…how many really<br/>are positive?"]
    end
  ```
- **Source/licence:** original. Alt text: two boxes showing the same relationship read in opposite directions.

## Slide 13 — 85% of offenders were gamers. So 0.02% of gamers are offenders.

- **On-slide text:** P(gamer | homicidal) = **85%** · P(homicidal | gamer) = **0.02%** · Out of 100,000: 19,000 gamers, 4.25 homicidal · **Never associate a common trait with an uncommon one.**
- **Speaker notes:** Do this by **counting bodies out of 100,000** on screen, never by manipulating the formula — it is faster, checkable in a meeting, and much harder to get wrong. Then the correction, which is better than the original: the same data honestly supports "4× more likely" (vs. the whole population) or "24× more likely" (vs. non-gamers) — and both describe 2 in 10,000. **A relative risk without an absolute risk is not information.** Ask always: multiplied from what, compared to whom? Transfer it to work: "80% of outages involved framework X" is exactly what you'd expect if X had no effect, when 80% of commits use X.
- **Visual:** The nested-set Mermaid from `content/04` §2, captioned "not to scale — and it cannot be, which is the lesson":
  ```mermaid
  flowchart TD
    A["Entire population<br/>100,000 people"] --> B["Play violent video games<br/>19,000 people"]
    A --> C["Do NOT play<br/>81,000 people"]
    B --> D["…and are homicidal<br/>4.25 people"]
    C --> E["…and are homicidal<br/>0.75 people"]
    D --> F["P(homicidal | gamer)<br/>= 4.25 / 19,000 = 0.02%"]
  ```
- **Source/licence:** original arithmetic (re-derived and corrected). Case structure from the DL Day 3 source deck — **LINK-ONLY**. Alt text: nested sets showing 4.25 homicidal people inside 19,000 gamers inside 100,000 population.

## Slide 14 — REVEAL 1: "99% of at-risk patients test positive" — 99% of *what*?

- **On-slide text:** That is **sensitivity**: P(positive | at risk) · Measured only on at-risk patients · A test that flags **everyone** scores 100% · **Ask: what is the precision?**
- **Speaker notes:** Return to the hook. Do not doubt the number — identify it. Read the vendor's sentence back and point at the population it describes. Then the killer aside: flag every patient and you have 100% sensitivity and a worthless test, so sensitivity alone can never be evidence. First question earned.
- **Visual:** The vendor's claim as a quote card with the words "who are at risk" highlighted.
- **Source/licence:** original.

## Slide 15 — REVEAL 2: precision is 79.8%, and that still isn't your answer

- **On-slide text:** 1,000 tested patients · At risk: 198 positive / 2 negative · Not at risk: 50 positive / 750 negative · Sensitivity 198/200 = **99% ✓** · Precision 198/248 = **79.8%** · Specificity 750/800 = 93.75%.
- **Speaker notes:** Vendor is cooperative; the matrix is real; the 99% checks out. 79.8% is a defensible screening tool and **this is where most evaluations stop and most purchases happen**. Then plant the unease: every number so far came from the vendor. You have verified their internal consistency and nothing else. What can you check independently? Exactly one thing — and it's public.
- **Visual:** The vendor's 2×2 confusion matrix, large, with the three derived metrics beneath.
- **Source/licence:** original arithmetic.

## Slide 16 — REVEAL 3: their sample was 20% at risk. The world is 1%.

- **On-slide text:** Vendor's test set: 200 / 1,000 = **20% at risk** · Real population: **1% at risk** · A 20× enrichment · **Precision is not a property of the test — it is a property of the test *and the population*.**
- **Speaker notes:** Be fair to the vendor here — enrichment is *standard, correct practice*. Validating a rare-condition test on an unenriched sample would need 20,000 patients. Nobody did anything wrong. But sensitivity and specificity travel with the test, and **precision does not**. Move the population and precision changes with not one line of the model changing. This is the slide the whole session exists for.
- **Visual:** Two stacked bars side by side — vendor sample (20/80) vs. real population (1/99) — at identical scale so the mismatch is visual, not arithmetic. Label both bars in text (greyscale-safe).
- **Source/licence:** original.

## Slide 17 — REVEAL 4: 99% → 3.39% (and the honest number is 13.8%)

- **On-slide text:** As the source states it: (.99 × .01) / .248 = **3.39%** · Worked correctly: 0.0099 / 0.0718 = **13.8%** · Per 100,000: 7,178 flagged, **6,188 of them wrong** · **Six of every seven positives are false alarms.**
- **Speaker notes:** Land 3.39% first — that's the reveal as the source stages it. Then correct it in front of the room, because this is the best two minutes in the deck: the source has an arithmetic slip (.0099/.248 = 3.99, not 3.39) **and** a deeper error — it reuses the vendor's 24.8% positive rate, which belongs to the vendor's 20% population. A deck teaching people not to mix populations mixed populations. Say that plainly. The honest 13.8% is *better* for the vendor and changes nothing. Then re-poll the room against the flipchart from slide 3.
- **Visual:** The 100,000-person counting table from `content/05` §5b — 990 / 6,188 / 7,178 — beside the two formulas. **This must not appear before this click.**
- **Source/licence:** original arithmetic; correction of the DL Day 3 source deck (**LINK-ONLY**) noted openly.

## Slide 18 — One unchanged test: precision from 1.6% to 79.8%

- **On-slide text:** Prevalence 0.1% → precision **1.6%** · 1% → **13.8%** · 5% → **45.5%** · 20% → **79.8%** · Flag rate barely moves: 6.3% → 24.8% · **Which row of this table are you standing on?**
- **Speaker notes:** The single most useful table in the session — tell people to photograph it. Same test throughout; only the population changes. The second column is the hidden lesson: alert *volume* moves barely fourfold while precision moves fiftyfold, so **a steady alert count is fully compatible with the tool having become worthless.** Then the AI-tooling swap: same arithmetic, defect-prediction tool, your repo at 1% — 718 flags per 10,000 commits, 619 false, a third of an engineer consumed, and within a fortnight the flags are closed unread. The dashboard still reports 99%, truthfully, every day.
- **Visual:** The prevalence-sweep table from `content/05`, with the 1% row highlighted, plus the 10,000-commit table as an inset or a second click.
- **Source/licence:** original arithmetic; scikit-learn/NumPy for the computation if code is shown (**BSD-3**).

## Slide 19 — P-hacking is usually not fraud. It is a deadline.

- **On-slide text:** *"No paper, no funding."* · *"Our client wants to see 10% savings."* · *"Our VC investors want a demonstration."* · Six techniques — the ML-specific one is **shopping for random seeds** · Defence: a validation set touched **once**.
- **Speaker notes:** Say the framing sentence deliberately — *not usually malicious; human nature under pressure and career survival* — because it decides whether the room becomes usefully sceptical or uselessly cynical. Read the middle quote again and point out it is a normal commercial instruction that nobody experiences as a p-hacking instruction. Then the operational consequence: if you think of it as fraud you look for bad actors, find none, and relax; if you think of it as incentive you look at the process and build controls. Close on the third split: the validation set is a controlled artefact — version it, restrict it, log every access. Paraphrase Coase; do not put the quotation on the slide.
- **Visual:** The three pressures as three cards, plus the six techniques as a compact list. No quotation marks around Coase.
- **Source/licence:** framing and the three pressures from the DL Day 3 source deck — **LINK-ONLY**, reworded. Ioannidis 2005 (PLoS) cited by reference on the resources slide.

## Slide 20 — Four questions. Take a photo.

- **On-slide text:** 1. That percentage — **of what population**? · 2. Precision, on what test set? Raw counts. · 3. Base rate of the positive class in **your** test set? · 4. Base rate in **our** population? ← *you answer this one* · Then: what happens when it's wrong, and who finds out?
- **Speaker notes:** The deliverable. Hand out the printed one-pager here. Reiterate that question 4 is the only one the vendor cannot answer for you, and it is the one that changes the decision. Close on the session's single sentence: *a number can be completely true and completely misleading, and the gap is the population it was measured in.*
- **Visual:** Full-bleed numbered card, large type, deliberately screenshot-friendly. Optionally the vendor decision flowchart from `content/05` on the facing half:
  ```mermaid
  flowchart TD
    S["Vendor quotes a<br/>headline accuracy number"] --> Q1{"Sensitivity, precision,<br/>or accuracy?"}
    Q1 -->|"Won't say"| NO["Walk away"]
    Q1 -->|"Identified"| Q2{"Full confusion matrix,<br/>raw counts?"}
    Q2 -->|"No"| NO
    Q2 -->|"Yes"| Q3{"Their base rate vs.<br/>my base rate?"}
    Q3 -->|"Similar"| OK["Their precision ≈ your precision.<br/>Proceed to a pilot."]
    Q3 -->|"Theirs is higher"| BAYES["Recompute precision<br/>at YOUR base rate"]
    BAYES --> Q4{"Can you afford that<br/>false-alarm volume,<br/>every day, forever?"}
    Q4 -->|"No"| NARROW["Narrow the population,<br/>stage the test, or don't buy"]
    Q4 -->|"Yes"| PILOT["Pilot — and measure<br/>precision on YOUR data"]
  ```
- **Source/licence:** original. Alt text: vendor-evaluation decision flowchart; both acceptable endings require measurement on your own data.

## Slide 21 — Discussion

- **On-slide text:** *"Where in our own reporting do we quote a number that is technically true and practically misleading?"* · plus the four prompts from `exercises/discussion.md`.
- **Speaker notes:** Open with the seed question and let it breathe — this room will have examples, and their examples are better than any you can supply. Prompts 2 (is our human-in-the-loop real?) and 4 (what's our base rate, and do we even know it?) are the ones most likely to produce an action item. Capture actions on the flipchart.
- **Visual:** Discussion/poll layout.
- **Source/licence:** none.

## Slide 22 — Resources & credits

- **On-slide text:** scikit-learn (BSD-3) · Maynez et al. 2020, ACL Anthology (CC BY 4.0) · RAGAS — Es et al., EACL 2024 (CC BY 4.0) · ARES — Saad-Falcon et al., NAACL 2024 (CC BY 4.0) · Ioannidis 2005, PLoS Medicine · Session lab + full source list in `resources/sources.md`.
- **Speaker notes:** Point at the lab for anyone who wants to run the numbers themselves — 25 minutes, Colab, no setup. Note that all statistics in this deck were re-derived for this course and that two source-deck errors were corrected on slides 13 and 17.
- **Visual:** Resources & credits layout with licence tags.
- **Source/licence:** attribution slide.

---

## Deck-builder checklist (in addition to `powerpoint_instructions.md` §5)

- [ ] Slides 14–17 are a **progressive reveal**. No number is visible before the question that earns it. If animation is unavailable, use separate slides and never a summary slide before 17.
- [ ] Slide 3's poll counts are recorded on a flipchart and revisited at slide 17.
- [ ] Slide 11's confusion matrix distinguishes cells by **label or shape**, not colour alone; readable in greyscale.
- [ ] Slide 16's two bars are at **identical scale**; both labelled in text.
- [ ] Slide 18 is photograph-friendly at the back of the room (≥ 20 pt in the table).
- [ ] No Coase / Ng / Chollet quotation appears in quotation marks anywhere. Paraphrase only.
- [ ] No source-deck figure, layout, or wording is reproduced. All diagrams are the Mermaid from `content/`.
- [ ] The corrections on slides 13 and 17 are presented as content, not as errata in small print.
- [ ] Alt text on every diagram, table and chart.
