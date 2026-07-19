# Sources — Session 13

Every source this session draws on, with a reuse verdict governed by the spec's licence discipline (`../../_TEMPLATE/SESSION_STRUCTURE.md` §4):

- **SLIDE-SAFE** — permissive / CC-BY / BSD / public-domain / standards body. May derive slide and content text and figures **with attribution**.
- **LINK-ONLY** — all-rights-reserved / NC / ND / vendor / internal. Reference it, assign it as reading, or show it as a live demo — **never reproduce it on a slide**.

When in doubt, treat as LINK-ONLY.

> **Note on the mathematics.** Every statistic, probability, confusion matrix and Bayes computation in this session was **re-derived and independently verified for this course** (verification run recorded in `../exercises/lab.md`). Mathematical facts are not copyrightable, and the specific numbers, tables, code and diagrams here are ours. The *scenarios* that carry them — the Michael parable, the medical-vendor role-play, the video-games case — originate in a LINK-ONLY source deck (#6) and have been **rebuilt in our own words, with our own numbers and two corrections**. Nothing from that deck is reproduced.

---

## Slide-safe (embeddable with attribution)

**#1 — scikit-learn.**
- URL: `https://scikit-learn.org/stable/modules/model_evaluation.html` · `https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html`
- Licence: **BSD 3-Clause** (code and documentation). Attribution required; derivation permitted.
- Used for: all confusion-matrix and metric code in `content/03`, `content/06` and `exercises/lab.md`; the `classification_report` output format; the metric formulas on slide 11; the `zero_division` / `UndefinedMetricWarning` behaviour discussed in `content/03` §4.
- **Reuse verdict: SLIDE-SAFE.** Footer attribution: "scikit-learn, BSD-3."
- *Note:* the source deck's `RFE(model, 2)` positional call is deprecated in current scikit-learn (`n_features_to_select=2` is now required). Not used in this session, but relevant if borrowing code from Sessions 3–8.

**#2 — NumPy.**
- URL: `https://numpy.org/` · Licence: **BSD 3-Clause**.
- Used for: array construction in the lab and content code blocks.
- **Reuse verdict: SLIDE-SAFE.**

**#3 — Maynez, Narayan, Bohnet & McDonald (2020), *On Faithfulness and Factuality in Abstractive Summarization*.**
- Venue: Proceedings of ACL 2020, via the **ACL Anthology**. URL: `https://aclanthology.org/2020.acl-main.173/` · arXiv: `https://arxiv.org/abs/2005.00661`
- Licence: **CC BY 4.0** (ACL Anthology standard for ACL 2020 proceedings).
- Used for: the **intrinsic vs. extrinsic hallucination** definitions in `content/01` §2 and slide 5. *Intrinsic* = the output contradicts the source; *extrinsic* = the output cannot be verified from the source. Also introduced in Session 1 — deduplicate the depth if sequencing 1 and 12 close together, and let Session 13 carry the *operational* consequence (different failure modes, different mitigations) rather than re-teaching the definitions.
- **Reuse verdict: SLIDE-SAFE.** On-slide: "Definitions: Maynez et al. 2020 (CC BY 4.0)."
- *Verify at delivery:* confirm the CC BY 4.0 notice still displays on the Anthology page.

**#4 — Es, James, Espinosa-Anke & Schockaert (2024), *RAGAS: Automated Evaluation of Retrieval Augmented Generation*.**
- Venue: EACL 2024 (System Demonstrations), via the **ACL Anthology**. URL: `https://aclanthology.org/2024.eacl-demo.16/`
- Licence: **CC BY 4.0**.
- Used for: `content/01` §3 and slide 6 — the claim that **groundedness/faithfulness is a measurable, automatable property**, evaluated by decomposing an answer into atomic claims and checking each against the retrieved context. Supports the practical instruction: *ask a vendor for a faithfulness number, not only an accuracy number.*
- **Reuse verdict: SLIDE-SAFE.** Attribute on-slide.

**#5 — Saad-Falcon, Khattab, Potts & Zaharia (2024), *ARES: An Automated Evaluation Framework for Retrieval-Augmented Generation Systems*.**
- Venue: NAACL 2024, via the **ACL Anthology**. URL: `https://aclanthology.org/2024.naacl-long.20/` · arXiv: `https://arxiv.org/abs/2311.09476`
- Licence: **CC BY 4.0**.
- Used for: `content/01` §3 and slide 6 — the three-axis decomposition (context relevance / answer faithfulness / answer relevance) and the point that a responsible evaluation framework reports **confidence intervals**, not a bare score.
- **Reuse verdict: SLIDE-SAFE.** Attribute on-slide.

**#8 — Ioannidis, J. P. A. (2005), *Why Most Published Research Findings Are False*.**
- Venue: PLoS Medicine 2(8): e124. DOI: `10.1371/journal.pmed.0020124`
- Licence: **PLoS publishes under CC BY** (verify the specific article notice at delivery — PLoS licensing has varied over time for older articles).
- Used for: the replication-crisis anchor in `content/06` §3 and slide 19.
- **Reuse verdict: SLIDE-SAFE**, but **cite rather than quote**. The argument is better paraphrased for a non-academic audience, and citing sidesteps any residual licence ambiguity on a 2005 article.

---

## Link-only (reference / assign / paraphrase — never embed)

**#6 — Nield, T. *Deep Learning for Beginners — Day 3* (O'Reilly Live Training, 2024-09).** §Testing & Validation (pp. 36–40) and §Validation Pitfalls (pp. 43–59).
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- **The origin of this session's three scenarios**, all rebuilt in our own words and numbers:
  - The **Michael accuracy parable** (p.36) and the confusion matrix worked on pp.37–38 → `content/03`.
  - The **violence/video-games base-rate case** (pp.44–49) → `content/04`.
  - The **medical-AI-vendor role-play** (pp.50–55) → `content/05`. This is the strongest teaching asset in the whole corpus, and its *staging* — claim, then question, then outside fact, then verdict — is what makes it work.
  - **P-values, the lady-tasting-tea experiment, p-hacking** (pp.56–59), including the six techniques and the three motivation quotes → `content/06`.
- **Corrections applied (house rule: never present a source-deck error as fact).** All three are taught openly, in front of the room:
  1. **`content/05` §5b — the vendor arithmetic, two problems.** The deck computes `(.99 × .01) / .248` and prints **.0339**; the expression actually evaluates to **.0399**. More importantly, the denominator `.248` is the positive rate **in the vendor's 20%-prevalence sample** and is applied to a 1%-prevalence population. Recomputing P(positive) honestly at a 1% base rate — `0.99×0.01 + 0.0625×0.99 = 0.0718` — gives **P(at risk | positive) = 13.79%**. The verdict is unchanged (six of seven positives are false alarms), and the correction is pedagogically valuable: a deck teaching people not to mix populations mixed populations.
  2. **`content/04` §4 — the relative-risk figures.** The deck states a non-gamer probability of `.005%` and a `4×` multiplier. `.005%` is in fact the **whole-population** rate; P(homicidal | non-gamer) computed from the deck's own priors is `0.00093%`, giving **≈24×** against non-gamers and **≈4.5×** against the whole population. We teach both, because the fact that identical data honestly supports 4× or 24× is a *better* lesson than either number. The deck's nested-set diagram also labels the gamer set 12,000/100,000, inconsistent with its stated 19% (= 19,000); we use 19,000 throughout.
  3. **`content/03` §2 — confusion-matrix orientation.** The deck presents predictions on the rows and transposes the FP and FN cell labels. All six of its computed metric values are nonetheless correct, because FP = FN = 1 here makes the transposition cancel. We use the scikit-learn convention (truth on rows) and state it explicitly, noting that with any other numbers the same slip would flip precision and recall.
- Also relevant but **deferred to other sessions**: the S-curve (pp.64–65) and the proof-of-concept-to-production gap (p.71) → Session 15; the Uber Tempe case study (pp.88–101) → Session 14; selection bias, outliers and adversarial attacks (pp.61–67) → Session 14.

**#7 — Nield, T. *LLM System Safety and Security* (O'Reilly Live Training, ~2023).** "Trust" (pp.33–38), "Containing the Human Factor" (p.78), "Containing the Operating Domain" (p.77), Hallucination (p.~50).
- Licence: all-rights-reserved (O'Reilly live-training material). **LINK-ONLY.**
- Used for, all paraphrased and attributed verbally as *framing*:
  - The **99%/1% startle-factor trap** — *"if an LLM is performing well 99% of the time, it becomes that much harder for the human to identify that 1%"*, and the observation that system-safety research has established humans are poor at catching infrequent errors from automated systems → `content/02`.
  - **Human-in-the-loop is necessary but not sufficient** — you must evaluate whether the person is *fit* to evaluate the output → `content/02` §4. The five-question test and the "could they have produced it themselves?" fitness test are **authored for this course**.
  - The **verification paradox** — *the more the LLM improves, the more work there is in verifying the output* → `content/01` §4.
  - The **two legitimate use conditions** (the user can easily verify, or truth is irrelevant) and **"you make a system safer by constraining it to do less"** → `content/01` §4–5.
  - **Hallucination as extrapolation into sparse, brittle space** → `content/01` §1.
- The **hazard triangle (HS/IM/TTO)** and the **operating-domain framework** are the property of Session 14; only the compact decision rule is borrowed here.

**#9 — Coase, R. H., *Essays on Economics and Economists*** (University of Chicago Press). The origin of the widely-quoted line about torturing data until it confesses.
- Licence: all-rights-reserved. **LINK-ONLY.**
- Used for: `content/06` §3 and slide 19 — **paraphrased only**. Do not place the quotation on a slide. Attribution of the *idea* to Coase is fine.

**#10 — Ng, A. — the Stanford-radiology data-drift interview** (IEEE Spectrum). URL: `https://spectrum.ieee.org/andrew-ng-xrays-the-ai-hype`
- Licence: all-rights-reserved (IEEE Spectrum). **LINK-ONLY.**
- Not used directly in Session 13 (it belongs to Session 15), but it is the same lesson in a different costume — a model measured in one hospital and deployed in another — and is worth a one-sentence verbal callback at slide 16. **Paraphrase; do not quote on a slide.**

**#11 — Fisher, R. A. (1935), *The Design of Experiments*** — the lady-tasting-tea experiment.
- Licence: the original text is not cleanly public-domain in all jurisdictions. **LINK-ONLY** — the *experiment* and its arithmetic (1/70) are facts and freely usable; do not reproduce Fisher's prose.
- Used for: the p-value worked example in `content/06` §1. The $\binom{8}{4} = 70$ computation is ours.

---

## Further reading (LINK-ONLY, high quality)

Assign these; do not slide them.

- **Ioannidis (2005)**, *Why Most Published Research Findings Are False* — PLoS Medicine. The foundational replication-crisis paper. Short, readable, and the argument generalises directly from medicine to model evaluation. **(Also #8 — SLIDE-SAFE by licence, but better assigned than slid.)**
- **scikit-learn User Guide, §3.4 Metrics and scoring** — `https://scikit-learn.org/stable/modules/model_evaluation.html`. The reference for anyone who has to read or write an evaluation report. BSD-3, so it is also safe to excerpt.
- **Wikipedia, *Base rate fallacy*** — genuinely good, with several worked examples beyond the two used here (notably the terrorist-detection and drug-testing variants). CC BY-SA, so attribution and share-alike apply if reproduced; easier to link.
- **Reason, J., *Human Error* (1990)** and the automation-complacency literature — the system-safety grounding for `content/02`. Also the origin of the Swiss cheese model used in Session 14.
- **Wachter-Boettcher / Narayanan & Kapoor, *AI Snake Oil*** — `https://www.aisnakeoil.com/`. The benchmark-scepticism thread that runs through Sessions 13, 14 and 15. Their "memorisation is a spectrum" framing is used in Session 9.
- **Simpson's paradox** — the natural next topic for anyone energised by `content/04`. A base-rate effect strong enough to reverse the direction of a result when groups are combined.
- **Wells, G. L. et al., on relative vs. absolute risk communication** — for anyone who has to *report* numbers rather than only read them.

---

## Verification log

The arithmetic in this session was executed and confirmed, not asserted:

| Claim | Verified value |
|---|---|
| Michael confusion matrix (sklearn, truth-on-rows) | `[[98, 1], [1, 0]]` |
| Michael metrics | sens 0.0, spec 0.9899, prec 0.0, NPV 0.9899, acc 0.98, F1 0.0 (undefined; `UndefinedMetricWarning`) |
| `classification_report` averages | macro 0.49, weighted 0.98, support 99/1 |
| Degenerate "nobody quits" model | accuracy 0.99 |
| Vendor precision at 20% prevalence | 0.7984 (= 198/248 ✓), positive rate 0.2480 (= 248/1000 ✓) |
| Vendor precision at 1% prevalence | **0.13793**, positive rate 0.07178 |
| Source deck's expression `.99×.01/.248` | **0.039919** (deck prints 0.0339) |
| P(homicidal \| gamer) | 0.00022368 = **0.0224%** |
| P(homicidal \| non-gamer) | 0.00000926 = **0.00093%**; ratio **24.16×** |
| Prevalence sweep (precision) | 0.1%→1.6% · 0.5%→7.4% · 1%→13.8% · 2%→24.4% · 5%→45.5% · 10%→63.8% · 20%→79.8% |
| Specificity needed at 1% prevalence | 99.0% for precision 50%; 99.89% for precision 90% |
| Defect tool at 1% base rate, 10,000 commits | 718 flagged, 99 real, 619 false (86%), ~239 investigation hours |

Anyone re-running these can reproduce them from `../exercises/lab.md` in under five minutes.
