# Slides — Session 3: Methods I, Learning From Data

Slide-by-slide spec for the deck-builder. Build per `../../powerpoint_instructions.md` (layout, palette, type, a11y, licence footers) — that file governs *how*; this governs *what*. Target: 1 title + 1 agenda + **15 content** + 1 Q&A + 1 resources = **19 slides**, ~45 minutes.

Licence quick-reference for this deck: all code/figures are **scikit-learn (BSD-3) → embeddable with attribution footer**. The **source deck (Deep Learning for Beginners, O'Reilly) is LINK-ONLY** — its ideas are paraphrased in our own words; nothing from it is reproduced. Every Mermaid below is original to this course and safe to render in-palette.

---

## Slide 1 — Title

- **On-slide text:** "Methods I: Learning From Data" · Session 3 of 16 · Block: Methods · AI Training Series.
- **Speaker notes:** This is the foundation for the next four sessions. Trees, forests, deep learning, LLMs all sit on the ideas we cover today, so we do them once, here. It's a concept session — one short code illustration, no full lab.
- **Visual:** Series title layout. No derived content.
- **Source/licence:** none.

## Slide 2 — Agenda

- **On-slide text:** the six segments with minute budget (Hook 0–4 · Supervised learning 4–12 · What a model is 12–19 · Holding data back 19–29 · Regression vs. classification 29–37 · Which model for which problem 37–45 · Q&A 45–60).
- **Speaker notes:** Flag that the "holding data back" segment is the one that matters most and the one we protect if we run long. 45 minutes is tight with no slack.
- **Visual:** agenda table, matches README.
- **Source/licence:** none.

## Slide 3 — The inversion (the hook)

- **On-slide text:** Headline: **"Machine learning runs software backwards."** · Traditional: Rules + Data → Answers · ML: Data + Answers → **Rules**.
- **Speaker notes:** The one idea to anchor the whole block. In normal software you write the rules; in supervised learning you supply examples *with* answers and the machine infers the rules. That's the power (solve problems whose rules you can't write) and the danger (the rules are only as good as the examples, and you often can't read them). Hold on this — it reframes everything after.
- **Visual:** the two-lane Mermaid from `content/00-overview.md` (Traditional vs. Supervised ML).
  ```mermaid
  flowchart LR
      subgraph TR["Traditional software"]
        R1["Rules"] --> P1["Program"]
        D1["Data"] --> P1
        P1 --> A1["Answers"]
      end
      subgraph ML["Supervised ML"]
        D2["Data (features)"] --> P2["Training"]
        A2["Answers (labels)"] --> P2
        P2 --> R2["Rules (the model)"]
      end
  ```
- **Source/licence:** original diagram; concept framing paraphrased from DL Day 1 (LINK-ONLY — do not quote the deck).

## Slide 4 — Features and labels

- **On-slide text:** Headline: **"Features go in, the label is what you predict."** · Feature = an input column · Label = the answer column · One row = one example.
- **Speaker notes:** Walk the colour table: R, G, B are features; light/dark is the label. To the model "dark" means nothing but a category attached to certain RGB combinations. Land the sharp question: *a model is only as good as its labels — where did they come from?*
- **Visual:** the 5-row RGB→label table from `content/01-supervised-learning.md`.
- **Source/licence:** scikit-learn-compatible toy data; table is original. Colour-problem *idea* from DL Day 1 (LINK-ONLY) — reproduced as our own table, not their slide.

## Slide 5 — The training loop

- **On-slide text:** Headline: **"Training is a loop, not a moment."** · Predict → measure error → adjust settings → repeat · Stops when it stops improving.
- **Speaker notes:** Every supervised method is a version of this loop; they differ only in what "adjust" means. Two things to plant for later: the model IS its settings (random at first → chance accuracy); and the loop only optimises the *training* data — which is exactly why we'll have to hold data back.
- **Visual:** the supervised-learning loop flowchart from `content/01-supervised-learning.md`.
  ```mermaid
  flowchart TD
      S["Model with random settings"] --> P["Predict on a batch of training examples"]
      P --> C["Compare to true labels → measure error"]
      C --> Q{"Still improving?"}
      Q -->|Yes| U["Adjust settings a little"]
      U --> P
      Q -->|No| D["Freeze settings = trained model"]
  ```
- **Source/licence:** original diagram.

## Slide 6 — What a model is (dark clouds)

- **On-slide text:** Headline: **"A model is a learned instinct: dark clouds mean rain."** · Structure (chosen) + settings (learned) · Features in → prediction out.
- **Speaker notes:** You already run models: nobody gave you a rain rulebook; you saw thousands of skies-then-outcomes and fit one. Draw the mapping — sky = features, "it'll rain" = prediction, years of weather = training data, instinct = learned settings. Two honest limits: you can be confidently wrong (dark sky, no rain), and you only know the weather where you've lived (drift).
- **Visual:** the weather↔ML mapping table from `content/02-what-is-a-model.md` (5 rows).
- **Source/licence:** original.

## Slide 7 — Training cost vs. inference cost

- **On-slide text:** Headline: **"Building a model is expensive; using it is cheap — per call."** · Training: once, all the data, many passes, GPUs · Inference: every call, one pass, milliseconds · At scale, inference dominates.
- **Speaker notes:** Studying for the exam vs. answering one question. Training touches every example many times; inference just plugs one input into a finished function. Consequence for this room: a cheap-to-train model can be expensive to run at volume; retraining isn't free; treat a deployed model as a re-qualifiable dependency, not a shipped-and-done artefact.
- **Visual:** the training-vs-inference cost table from `content/02-what-is-a-model.md` (condense to 4–5 rows), or the two-box Mermaid.
- **Source/licence:** original. Ties to Session 2 token-cost lesson (inference = per-use billing).

## Slide 8 — The trap: memorising the exam

- **On-slide text:** Headline: **"Accuracy on training data is a memorised-exam score."** · A model can memorise, not learn · The only score that counts is on **unseen** data.
- **Speaker notes:** The training loop rewards memorising — a big model can build a lookup table and score 100% on data it's seen, having learned nothing transferable. Analogy: a student who memorised last year's paper. The only meaningful test is this year's paper — questions they've never seen. This sets up the split.
- **Visual:** simple two-panel — "Trained on it: 100%" vs "Never seen it: ?". Optional overfit curve (train accuracy up, test accuracy peaks then falls).
- **Source/licence:** original; "memorised exam" is a standard analogy, our wording.

## Slide 9 — Overfitting, made visible

- **On-slide text:** Headline: **"Overfitting is invisible until you look at held-out data."** · Underfit (bad/bad) → Good fit (good/good) → Overfit (great/bad) · Training accuracy alone hides it.
- **Speaker notes:** As a model gets more complex or trains longer, training accuracy keeps rising, but unseen-data accuracy rises, peaks, then falls. The gap between the curves is the overfitting. If you only watch training accuracy you never see the peak and you ship the overfit model.
- **Visual:** the underfit→good→overfit Mermaid from `content/03-train-val-test-split.md`; or an `xychart-beta` of train vs. test accuracy over complexity (two lines, distinguished by label + dash pattern, not colour alone).
- **Source/licence:** original.

## Slide 10 — The 70/15/15 split

- **On-slide text:** Headline: **"Split the data three ways before you train."** · Train ~70% — fit the settings · Validation ~15% — tune the choices · Test ~15% — the one-time report card.
- **Speaker notes:** Non-negotiable discipline. Train fits parameters; validation tunes hyperparameters and model choice; test is the final honest number. Percentages are a convention, not a law.
- **Visual:** the 70/15/15 split flowchart from `content/03-train-val-test-split.md`.
  ```mermaid
  flowchart LR
      ALL["All labelled data"] --> TR["Training ~70%"]
      ALL --> VA["Validation ~15%"]
      ALL --> TE["Test ~15%"]
      TR --> U1["Fit the settings"]
      VA --> U2["Tune the choices"]
      TE --> U3["Final report card — once"]
  ```
- **Source/licence:** original. 70/15/15 is standard practice (source deck used a 2-way 2/3–1/3 split; we add validation and say why).

## Slide 11 — Why three, and the golden rule

- **On-slide text:** Headline: **"Tune on validation. Touch the test set once."** · Picking the best model *by the test set* leaks it into your choices · Validation absorbs the tuning · **Test set: touched exactly once, at the very end.**
- **Speaker notes:** Two sets stops you grading your own homework; but if you tune your *choices* to the test set, it's contaminated again — a slower memorisation. Validation is the sacrificial set you iterate against so test stays sealed. Map to Session 2: training learns parameters, validation guides hyperparameters, test judges the finished result. The golden rule belongs on a wall.
- **Visual:** callout box of the golden rule + the parameters/hyperparameters/finished-result mapping (3-row mini-table).
- **Source/licence:** original.

## Slide 12 — The split in code (demo)

- **On-slide text:** Headline: **"Splitting is one function call — and it exposes the gap."** · `train_test_split(..., stratify=y, random_state=42)` · `model.score(train)` 1.00 vs `model.score(test)` 0.82.
- **Speaker notes:** Show the scikit-learn call. Two flags that aren't decoration: `random_state` makes the split reproducible (config-management point); `stratify` keeps class proportions equal across sets. Then the punchline: the trained model scores ~1.00 on train, ~0.82 on test — that gap IS the overfitting, and 0.82 is the only number you'd ever quote. Pre-run this; do not live-type. Full version in the lab file.
- **Visual:** the two code snippets from `content/03-train-val-test-split.md` (split + the score comparison). Footer tag: "scikit-learn, BSD-3".
- **Source/licence:** **scikit-learn (BSD-3) — SLIDE-SAFE**, attribute in footer.

## Slide 13 — Regression vs. classification

- **On-slide text:** Headline: **"Number out = regression. Label out = classification."** · "How much?" vs. "Which one?" · Look at the label to tell which you have.
- **Speaker notes:** Both supervised; the only difference is whether the label is a value on a scale or a choice from a set. Self-test: "how many days until this disk fails?" = regression; "will it fail in 30 days?" = classification. The second is often more useful because it ends in a decision.
- **Visual:** the regression-vs-classification comparison table from `content/04-regression-vs-classification.md` (the required comparison table — render it as a proper on-slide table, ≥18pt).
- **Source/licence:** original table; concept from DL Day 1 (LINK-ONLY), reworded.

## Slide 14 — A probability becomes a decision

- **On-slide text:** Headline: **"A classifier outputs a probability; a threshold makes the call."** · Model → P(dark) = 0.73 · Rule: P ≥ 0.5 → dark, else light.
- **Speaker notes:** The step everyone skips. The model doesn't jump to "dark" — it computes a probability (sigmoid squeezes to 0–1), then a cut-off converts it to a class. Walk the little P-table: 0.02→light, 0.73→dark. The mechanism behind "the AI decided" is one number and one comparison.
- **Visual:** the P(dark)→decision table (6 rows) from `content/04-regression-vs-classification.md`.
- **Source/licence:** original.

## Slide 15 — The threshold is a business choice

- **On-slide text:** Headline: **"0.5 is a default, not a law — moving it trades your errors."** · Lower threshold → catch more, more false alarms · Raise → fewer alarms, miss more · Which error is worse is *your* call.
- **Speaker notes:** Risky-config-change classifier: false negative = bad change ships (incident); false positive = a wasted review. Not equal — so you lower the threshold to catch more true risks at the cost of more reviews. Spam filter moves it the other way. The sharp vendor question: "what's the threshold, who chose it, did they weigh FP vs FN?" Many systems ship 0.5 by accident. (Precision/recall in Session 8; base rates in Session 13.)
- **Visual:** the threshold trade-off Mermaid from `content/04-regression-vs-classification.md`.
- **Source/licence:** original.

## Slide 16 — Correct the source-deck error (credibility beat)

- **On-slide text:** Headline: **"Always pin down what the probability is *of*."** · Source deck said ≥0.5 → dark on one slide, ≥0.5 → light on another · We fix it: output = P(dark); ≥0.5 → dark, <0.5 → light.
- **Speaker notes:** Short honesty beat. One of our source decks contradicts itself on exactly this threshold — a bug a technical audience would catch. We correct it and generalise the lesson: a probability of 0.7 is useless until you know "0.7 of what?" Mislabelling which class the probability refers to is one of the commonest quiet bugs in real classifiers. (Full note in AI_input.md §6, error #1.)
- **Visual:** before/after callout: "contradiction ✗" vs "P(dark), ≥0.5→dark ✓".
- **Source/licence:** correction of DL Day 1 (LINK-ONLY) — we describe the error in our own words; we do not show their slides.

## Slide 17 — The decision heuristic (the keeper)

- **On-slide text:** Headline: **"Structured data → simple model. Perceptual data → neural network."** · Tabular (rows/columns) → regression, tree, forest · Images/audio/text → neural network · Start simple; escalate only when forced.
- **Speaker notes:** The tool to walk out with. Neural nets win on perceptual problems because they learn their own features from raw signal — wasted on tabular data whose features are already meaningful columns. Walk two of the worked judgements (risky-config-change = tabular = tree; screenshot-classification = perceptual = neural net).
- **Visual:** the which-model decision flowchart from `content/05-which-model-for-which-problem.md`.
  ```mermaid
  flowchart TD
      START["A prediction problem. What is the data like?"] --> Q1{"Structured / tabular?"}
      Q1 -->|Yes| SIMPLE["Simple model first"]
      Q1 -->|"No — perceptual/fuzzy"| NN["Neural network likely justified"]
      SIMPLE --> Q2{"Meets the requirement?"}
      Q2 -->|Yes| DONE["Ship it — cheaper, auditable"]
      Q2 -->|No| NN
  ```
- **Source/licence:** original diagram; "when to use NNs" framing paraphrased from DL Day 1 (LINK-ONLY).

## Slide 18 — Use the simplest model that works

- **On-slide text:** Headline: **"When all you have is a hammer, everything looks like a nail."** · Neural nets cost: more data · more compute · opacity · harder maintenance · Auditable can beat marginally-more-accurate.
- **Speaker notes:** The discipline note. Real pull toward deep learning first — resist it. The four costs land on your side of the house. For an accountability-driven room, opacity is decisive: a tree you can read and defend often beats a black box you can't. Most release/problem/config problems are tabular → simple-model problems; reserve and budget neural nets for genuinely perceptual cases. Bridge: this maps the block — S4 unsupervised, S5 the simple/auditable branch, S6–8 the neural branch, S9 LLMs.
- **Visual:** the four-costs `graph LR` from `content/05-which-model-for-which-problem.md`.
- **Source/licence:** original; "hammer/nail" is a common aphorism (not a deck quote), our wording.

## Slide 19 — Q&A / discussion

- **On-slide text:** "Questions & discussion (15 min)" + 2–3 seed prompts (e.g. "Name a problem on your team — is it tabular or perceptual?"; "Where might a model be graded on its own homework?").
- **Speaker notes:** Run the poll from `exercises/discussion.md`. Steer toward participants' own systems.
- **Visual:** discussion layout.
- **Source/licence:** none.

## Slide 20 — Resources & credits

- **On-slide text:** scikit-learn (BSD-3) — code & the split demo · Deep Learning for Beginners, O'Reilly (link-only, assigned as reading) · Session 3 reading list.
- **Speaker notes:** Point to `content/` for self-study and `resources/sources.md` for the full licence verdicts.
- **Visual:** resources/credits layout with licence attributions.
- **Source/licence:** attributions per `../resources/sources.md`.

---

### Build checklist for this deck
- [ ] 15 content slides (3–18); every headline is a claim, not a topic.
- [ ] Three required diagrams present and in-palette: supervised-learning loop (S5), which-model decision flowchart (S17), regression-vs-classification comparison table (S13).
- [ ] Only the code slide (S12) carries embedded derived material — footer "scikit-learn, BSD-3". No O'Reilly-deck content reproduced anywhere.
- [ ] Alt text on every diagram; no meaning by colour alone (the two overfitting/threshold lines need label + shape, not just colour); 18 pt min; greyscale-safe.
- [ ] Speaker notes on every content slide.
