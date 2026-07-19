# Slides — Session 5: Decision Trees & Random Forests

Deck spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, a11y). **Target: 16 content slides** + title + agenda + Q&A + resources = 20 slides, ~45 min. Speaker notes carry the detail; slides stay sparse. Every derived visual is scikit-learn (BSD-3) — tag its footer `scikit-learn, BSD-3`. Mermaid sources are provided so diagrams can be rendered in-palette. **No LINK-ONLY material is embedded** (r2d3, StatQuest are live-demo/reading only).

---

## Slide 1 — Title
- **On-slide:** "Methods III — Decision Trees & Random Forests" · Session 5 · Block: Know the Methods · "The model you can read."
- **Speaker notes:** Frame the session as the deliberate opposite of the black-box LLM. This is the one mainstream method whose reasoning you can read end to end — which is exactly what release/problem/config work needs. We'll build a tree, see how it thinks, then make it reliable with a forest.
- **Visual:** Series master title layout.
- **Source/licence:** none.

## Slide 2 — Agenda
- **On-slide:** the 7-row agenda from `README.md` (Hook → flowchart → Gini → overfit → forest → why-it-matters → recap).
- **Speaker notes:** Flag the Gini segment as the arithmetic-heavy stretch; promise it's one formula reused. 45 min + 15 Q&A.
- **Visual:** Agenda layout; minute budget matches README.
- **Source/licence:** none.

## Slide 3 — Hook: the model that shows its work
- **On-slide headline (a claim):** "Every other model gives you an answer. A tree gives you the reason."
- **Bullets:** LLM: fluent answer, no readable why · Tree: read every decision · For audit/root-cause/review, that's the point.
- **Speaker notes:** Ask the room: when a model tells you something, can you defend it in a change review? With most models, no. With a tree, you hand over the exact rule path. That difference is the whole session. Don't oversell accuracy — trees aren't the most accurate; they're the most *readable*.
- **Visual:** interpretability spectrum (Mermaid `graph LR`: tree → forest → linear → NN → LLM). Redraw in palette; label "readable ➜ opaque" with shape, not colour alone.
- **Source/licence:** original diagram.

## Slide 4 — A tree is a learned flowchart
- **On-slide headline:** "A decision tree is a flowchart the machine writes itself."
- **Bullets:** root = first question · internal = follow-ups · leaf = prediction · predict = walk root→leaf · the machine picks the questions, not you.
- **Speaker notes:** Everyone here has followed a troubleshooting runbook — same structure. The new idea: the machine *learns* the questions and their order from data. No arithmetic to predict — just follow signs.
- **Visual:** anatomy Mermaid (`flowchart TD`, root→internal/leaf) from `content/01`.
- **Source/licence:** original diagram.

## Slide 5 — Worked example: "will they buy a computer?"
- **On-slide headline:** "14 past customers → a flowchart that predicts the next one."
- **Bullets:** 4 features (age, income, student, credit) · label = buys? · 9 yes / 5 no · goal: predict an unseen customer.
- **Speaker notes:** Walk the table briefly. This is the canonical teaching set (Han & Kamber). We'll compute the actual splits next slide, by hand. Keep it on screen — we build on it for three slides.
- **Visual:** the 14-row table from `content/01` (small original table).
- **Source/licence:** dataset is a standard textbook example; table is our own rendering — safe.

## Slide 6 — The tree the machine learns
- **On-slide headline:** "Read it as English: middle-aged always bought; youth depends on student; seniors on credit."
- **Bullets:** one path per customer · different customers, different questions · note the counter-intuitive senior/credit rule.
- **Speaker notes:** Walk one prediction live: senior + fair credit → yes, and we can say *why*. Flag the senior/excellent-credit → no branch as exactly the kind of odd pattern you can *see* only because the model is readable. Hold that for the interpretability slide.
- **Visual:** the buys-computer tree Mermaid (`flowchart TD`) from `content/01`.
- **Source/licence:** original diagram.

## Slide 7 — How does it choose the first question?
- **On-slide headline:** "It picks the question that makes the groups least *mixed*."
- **Bullets:** need a score for "mixed" · that score = Gini impurity · same cost/distance idea as everywhere in this course.
- **Speaker notes:** Set up the spine: regression minimises MSE, classification minimises log-loss, trees minimise Gini. One recurring idea — "reduce the cost" — new costume. This de-mystifies the whole method: it's a cost-minimiser like the rest.
- **Visual:** the cost/distance spine table (method → its cost) from `content/02`.
- **Source/licence:** original; concept framing paraphrased (Cisco deck excluded — do not cite it).

## Slide 8 — Gini impurity, defined
- **On-slide headline:** "Gini = 1 − Σ pᵢ² : 0 is pure, 0.5 is a coin-flip."
- **Bullets:** = chance of mislabelling a random pick · pure group → 0 · 50/50 → 0.5 (most costly).
- **Speaker notes:** Give the one-breath intuition: it's how often you'd be wrong labelling by the group's own mix. Correct the source-deck error out loud — a pure split is Gini **0**, not 0.5. Show the small table of makeups → Gini.
- **Visual:** the "group makeup → Gini" table from `content/02`.
- **Source/licence:** original.

## Slide 9 — Scoring the root split (the money slide)
- **On-slide headline:** "age wins: its children are the least mixed (weighted Gini 0.343)."
- **Bullets:** root Gini 0.459 · age → 0.343 · student 0.367 · credit 0.429 · income 0.440 · lowest wins.
- **Speaker notes:** Walk the age split: youth 0.48, middle-aged 0.00 (pure!), senior 0.48, weighted 0.343. That beats all others, so age is the root — the machine tried all four and this cut the cost most. This is the entire decision rule, once.
- **Visual:** the age-split child table + the four-feature scoreboard, both from `content/02`.
- **Source/licence:** original computation.

## Slide 10 — One tree memorises
- **On-slide headline:** "Grown freely, a tree scores 100% on training — and falls over in production."
- **Bullets:** splits until every leaf is pure · low bias, HIGH variance · train 1.00 / test 0.91 · the classic lab-vs-prod gap.
- **Speaker notes:** This audience knows "great in the demo, worse live." A full-grown single tree is the archetype. Two swapped rows can flip a branch — high variance. You can limit depth, but there's a stronger fix: average many trees.
- **Visual:** bias–variance shallow→deep→memorised Mermaid (`flowchart LR`) from `content/03`; overlay the 1.00/0.91 numbers from the demo.
- **Source/licence:** numbers from scikit-learn demo — tag `scikit-learn, BSD-3`.

## Slide 11 — The fix: grow many *different* trees
- **On-slide headline:** "Average many trees that make *different* mistakes, and the noise cancels."
- **Bullets:** needs diversity · trick 1: bootstrap (different data) · trick 2: random features per split · then vote.
- **Speaker notes:** Averaging only helps if the trees err differently. Two independent randomisers create that: bootstrap sampling and per-split feature subsets. Define bagging = Bootstrap AGGregatING. This is a random forest.
- **Visual:** the forest Mermaid (`flowchart TD`, data → bootstraps → trees → vote) from `content/03`.
- **Source/licence:** original diagram.

## Slide 12 — Bootstrap → the free 37%
- **On-slide headline:** "Sample with replacement, and ~37% of rows sit out each tree."
- **Bullets:** bootstrap = N draws, with replacement · ~63% used, ~37% left out · (1−1/N)^N → 1/e · left-out = out-of-bag.
- **Speaker notes:** Explain replacement: some rows twice, some never. The never-picked ~37% is out-of-bag and becomes free validation next slide. This is the one bit of arithmetic behind OOB.
- **Visual:** simple xychart or callout of (1−1/N)^N → 0.368; or reuse the forest diagram highlighting one bootstrap.
- **Source/licence:** original.

## Slide 13 — Out-of-bag error: validation for free
- **On-slide headline:** "Score each row using only the trees that never saw it — no holdout needed."
- **Bullets:** per row, ask its OOB trees · their vote = honest prediction · average = OOB error · one flag: `oob_score=True`.
- **Speaker notes:** This is the practical payoff of the 37%. OOB usually tracks a real test score closely; in the demo OOB 0.96 vs test 0.97. Caveat: noisy with few trees — report a proper test number when it counts.
- **Visual:** OOB Mermaid (`flowchart LR`, row → its OOB trees → vote → compare) from `content/03`.
- **Source/licence:** original diagram; numbers `scikit-learn, BSD-3`.

## Slide 14 — Forest beats tree — at a price
- **On-slide headline:** "The forest wins accuracy and stability — and loses the readable path."
- **Bullets:** test 0.91 → 0.97 · variance averaged away · but you can't read 100 trees · only feature importances survive.
- **Speaker notes:** This is the central trade of the session. A forest is more accurate and stable but you've traded away the per-prediction reasoning that made one tree special. Set up interpretability as the deciding factor.
- **Visual:** the tree-vs-forest comparison table from `content/03`.
- **Source/licence:** original; demo numbers `scikit-learn, BSD-3`.

## Slide 15 — What survives: feature importances (with caveats)
- **On-slide headline:** "You lose the path, you keep the ranking — but don't over-trust it."
- **Bullets:** importances = total Gini removed per feature · MDI biased to high-cardinality features · prefer permutation importance for real claims.
- **Speaker notes:** Feature importances are a genuine model-level explanation, but the built-in MDI over-rates high-cardinality features. For anything you'll defend, use permutation importance on held-out data. Honesty here matches the course voice.
- **Visual:** the demo's importance ranking (Part 4) rendered as a bar chart from scikit-learn output.
- **Source/licence:** `scikit-learn, BSD-3`.

## Slide 16 — Interpretability, made concrete for your role
- **On-slide headline:** "An explanation you can check beats an answer you have to trust."
- **Bullets:** justify a change · root-cause a bad call · satisfy an auditor · catch a spurious rule before prod · let an expert object.
- **Speaker notes:** Convert "interpretable" into deliverables this room already produces. The senior/credit branch is the live example: readable → a human can question it. An LLM gives a fluent story that may not be the real reason; a tree's explanation *is* its mechanism.
- **Visual:** the "you need to… / with a tree you can…" table from `content/04`.
- **Source/licence:** original.

## Slide 17 — When to reach for which
- **On-slide headline:** "Tree first when it must be defensible; forest for accuracy behind a human gate; black box only when the problem is perceptual."
- **Bullets:** must justify each decision? → tree · need accuracy + can review? → forest · images/text/signals? → NN/LLM + verify.
- **Speaker notes:** Give the honest counter-argument too: a single tree is often less accurate; a forest already sacrifices most readability; interpretable ≠ correct/fair. Interpretability is a tool for oversight, not a guarantee.
- **Visual:** the decision Mermaid (`flowchart TD`) from `content/04`.
- **Source/licence:** original diagram.

## Slide 18 — Live demo cue (optional, 2 min)
- **On-slide headline:** "See a tree think — and confirm the hand-maths in code."
- **Bullets:** scikit-learn: root Gini 0.4592, root = age · one tree overfits · forest + OOB · `plot_tree`.
- **Speaker notes:** Either run `exercises/lab.md` Parts 1–3 live, or show the r2d3.us visual intro as a live demo (LINK-ONLY — run it, do not screenshot). Fallback: static `plot_tree` figure (BSD-3, embeddable).
- **Visual:** live notebook OR r2d3.us (link-only, live only). Fallback: scikit-learn `plot_tree` PNG (BSD-3, embeddable).
- **Source/licence:** scikit-learn `plot_tree` `scikit-learn, BSD-3`; r2d3 **live-demo/link only — do not embed**.

## Slide 19 — Recap / key takeaways
- **On-slide headline:** "A tree shows its work; a forest makes it reliable; you choose which you need."
- **Bullets:** Gini = 1−Σp² (0 pure, 0.5 mixed) · one tree overfits · forest = bootstrap+vote, OOB free · tree = readable, forest = accurate.
- **Speaker notes:** Land the one-thing line: an explanation you can check beats an answer you have to trust. Point to `content/99` for the recap.
- **Visual:** the key-numbers table from `content/99`.
- **Source/licence:** original.

## Slide 20 — Q&A / discussion + resources
- **On-slide:** discussion prompts (from `exercises/discussion.md`) + resources with licence tags (from `resources/sources.md`).
- **Speaker notes:** Open with the "where would an unreadable model be unacceptable in your work?" prompt. List scikit-learn (BSD-3), and the link-only reading (StatQuest, r2d3).
- **Visual:** Discussion/poll layout, then resources/credits layout.
- **Source/licence:** attributions per `resources/sources.md`.

---

### Deck-builder checklist (this session)
- [ ] 16 content slides (3–18) + title/agenda/recap/Q&A = 20. Within the 16–24 target.
- [ ] Every headline is a claim, not a label.
- [ ] Diagrams rendered from the Mermaid in `content/*` and this file, in palette, with alt text.
- [ ] No red/green-only distinctions (bias–variance, readable→opaque use shape + label).
- [ ] Every derived number/figure tagged `scikit-learn, BSD-3`; r2d3 & StatQuest **never embedded**.
- [ ] Speaker notes on every content slide.
- [ ] Correct the source-deck Gini error on Slide 8 (pure = 0, not 0.5).
