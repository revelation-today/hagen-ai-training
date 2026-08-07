# Slides — AI in 30 Minutes (standalone intro)

Slide-by-slide spec. Build per `../../powerpoint_instructions.md` (layout, palette, type, accessibility, licence footers). Speaker notes go in the Notes pane, never on the slide. Every headline is a claim, not a label.

**Deck size:** 1 title + 1 agenda + 12 content + 1 takeaways + 1 Q&A/resources = **16 slides**, ~30 min (≈2 min/slide, with the demo slide taking longer).

**Licence quick-reference:** all diagrams, tables and examples here are **original to this course** and safe to render. The only named external source is Maynez et al. 2020 (CC BY 4.0, SLIDE-SAFE) for the hallucination definition, if you show it. 3Blue1Brown and any vendor blogs are LINK-ONLY — mention, don't embed.

---

## Slide 1 — Title

- **On-slide text:** "AI in 30 Minutes" · A plain-English introduction · AI Training Series.
- **Speaker notes:** Set expectations: no maths, no code, no jargon left undefined. By the end you'll understand what these tools are, why they sometimes lie, and how to get good results. One promise: you'll leave with a single mental model that makes the rest make sense.
- **Visual:** Title layout.
- **Source/licence:** none.

## Slide 2 — What we'll cover

- **On-slide text:** What AI is · How it basically works · Why it makes things up · How to work with it · How to write a good prompt.
- **Speaker notes:** Five beats, each builds on the last. Flag that beat three — why it makes things up — is the one that changes how you use these tools, and that beats four and five are the practical payoff.
- **Visual:** ```mermaid
flowchart LR
    A["What AI is"] --> B["How it works"] --> C["Why it lies"] --> D["Working with it"] --> E["Good prompts"]
```
- **Source/licence:** none.

## Slide 3 — You have been confidently, vividly wrong

- **On-slide text:** "Ever remembered something vividly — and been completely wrong?" · It didn't feel like a guess. · You couldn't tell from the inside.
- **Speaker notes:** Show of hands, take one 20-second story. Land the point gently: the memory felt true. That inability to feel your own error is the best intuition for how AI works and fails — we'll come back to it. This is the emotional anchor of the whole talk; don't rush it.
- **Visual:** Full-bleed question, minimal text.
- **Source/licence:** none.

## Slide 4 — AI turns programming inside out

- **On-slide text:** Classical software: data + rules → answers · Machine learning: data + answers → rules · Nobody writes the rule — the machine infers it.
- **Speaker notes:** The core idea in one flip. Classical software applies rules a human wrote. Machine learning is handed the data and the answers and works out the rules itself. Those "rules" are a pile of numbers we call a model. Everything else today is a consequence of this.
- **Visual:** ```mermaid
flowchart LR
    subgraph CL["Classical software"]
      D1["Data"] --> P1["Rules a human wrote"] --> A1["Answer"]
    end
    subgraph ML["Machine learning"]
      D2["Data"] --> P2["Learning algorithm"]
      A2["Answers (labelled)"] --> P2 --> R2["Rules = a model"]
    end
```
- **Source/licence:** original diagram.

## Slide 5 — Some rules can't be written, so we learn them

- **On-slide text:** Write the exact rule for "cat or dog in a photo." · You can't. A three-year-old can. · Fuzzy, perceptual tasks → learn from examples.
- **Speaker notes:** The cat/dog impossibility is the intuition pump. This is the whole reason machine learning exists: problems where examples are plentiful but rules can't be spelled out. Contrast with a tax calculation, where the rules are known and you'd never use AI.
- **Visual:** Two panels: "Rules you can write" (tax, sorting) vs. "Rules you can't" (recognise a face, judge tone, spot a defect).
- **Source/licence:** original.

## Slide 6 — Four words, nested — not the same thing

- **On-slide text:** AI ⊃ Machine Learning ⊃ Deep Learning ⊃ LLM · Every LLM is AI; most "AI" in a pitch is not an LLM · The tools you use are LLMs.
- **Speaker notes:** Clear up the vocabulary once. These are nested, not synonyms. When a vendor says "AI," ask which layer they mean. The tools this room touches — ChatGPT, Claude, Copilot — are all LLMs, so that's our focus from here.
- **Visual:** ```mermaid
flowchart TD
    AI["Artificial Intelligence"] --> ML["Machine Learning"] --> DL["Deep Learning"] --> LLM["Large Language Model"]
```
- **Source/licence:** original.

## Slide 7 — An LLM is autocomplete on steroids

- **On-slide text:** Trained by one game, played billions of times: guess the next word · At scale, it gets very good at plausible continuations · It generates text — it does not look things up.
- **Speaker notes:** This is the heart of the talk. The model learned by covering the next word and guessing, over and over, on a huge amount of text. When you ask it something, it isn't consulting a database — it's producing the most plausible next chunk of text, one piece at a time. Say the phrase and make them remember it: autocomplete on steroids.
- **Visual:** ```mermaid
flowchart LR
    P["Your prompt"] --> M["Predict most likely next token"] --> T["Append"] --> M
    T --> O["...until done"]
```
- **Source/licence:** original.

## Slide 8 — It's a pattern-matcher, not a search engine

- **On-slide text:** No database of facts inside · A statistical sense of what text usually looks like · True facts come out because they're common in the text — truth is a side effect · Works in tokens (~¾ of a word) — also the unit of billing.
- **Speaker notes:** Drive home the distinction — it's the root of everything in the next section. It doesn't retrieve; it reconstructs. Correct facts appear because true statements dominate the training text, not because it's checking anything. Mention tokens briefly: how it reads, writes, and how the paid APIs bill. Optional: temperature as the randomness dial.
- **Visual:** Two-column: "Search engine — retrieves a stored fact" vs. "LLM — generates a plausible continuation".
- **Source/licence:** original.

## Slide 9 — Hallucination is not a bug

- **On-slide text:** When it states something false as fact = a hallucination · It's a direct result of how the model works · Dense training data → usually right · Sparse data → fluent but possibly invented.
- **Speaker notes:** The most important slide for anyone who'll rely on these tools. The model's job is a plausible continuation. Where the pattern is well-covered, plausible is usually also true. Where it's thin — a niche fact, a specific number, something recent — it still produces a confident, fluent answer with nothing solid underneath. This will not be "fixed"; it's structural. It can only be managed.
- **Visual:** ```mermaid
flowchart LR
    P["Prompt"] --> R{"Well covered by<br/>training data?"}
    R -->|"Yes — dense"| G["Fluent, usually correct"]
    R -->|"No — sparse"| H["Fluent, possibly invented"]
    G --> S["Identical confidence.<br/>No warning label."]
    H --> S
```
- **Source/licence:** original. (Definitions after Maynez et al. 2020, CC BY 4.0 — attribute if you show the formal terms.)

## Slide 10 — Confidence tells you nothing about truth

- **On-slide text:** Same fluency whether right or invented · "It sounded confident" = zero information · Same failure as a false memory — and as prejudice · Never treat a confident answer as a verified one.
- **Speaker notes:** Callback to the opening. Your brain hands you a false memory feeling certain; the model hands you an invented fact feeling fluent — same mechanism, pattern-completion ahead of evidence. When it's about facts we call it hallucination; about people, bias. The practical line: confidence is free, verification is your job.
- **Visual:** Table — "Looks like / Actually is" (looking up → generating; confidence=correct → confidence=fluency; glitch → structural property).
- **Source/licence:** original.

## Slide 11 — One rule for when to trust it

- **On-slide text:** Use it when you can EASILY CHECK the answer · …or when TRUTH DOESN'T MATTER (drafts, ideas, tone) · Be careful exactly where neither holds · And always: you own the output.
- **Speaker notes:** The single most useful rule in the talk. Verifiable, or truth-irrelevant — those are the safe zones. The danger zone is an unverifiable answer that has to be right. The habit that follows: a human who can actually judge the result stays in the loop. A plausible answer in a field you don't know is the most dangerous kind.
- **Visual:** ```mermaid
flowchart TD
    Q{"Can you easily<br/>verify it?"}
    Q -->|Yes| OK["Good use"]
    Q -->|No| Q2{"Does truth<br/>matter here?"}
    Q2 -->|No| OK2["Fine — truth isn't the point"]
    Q2 -->|Yes| D["Danger — need an expert<br/>or a grounded source"]
```
- **Source/licence:** original.

## Slide 12 — See it happen (live demo)

- **On-slide text:** Live: ask it for a short bio of someone in the room · Watch it invent plausible, specific, wrong details · Confidence unchanged throughout.
- **Speaker notes:** DEMO. Ask the model for a biography of a colleague (or yourself) who isn't famous. It will produce fluent, specific, confident, largely fabricated detail. Nothing lands "confident and wrong look identical" harder than watching it happen live. Fallback if no network: a pre-captured screenshot. Keep it light — the point is visceral, not a gotcha.
- **Visual:** Live browser, or a fallback screenshot of a hallucinated bio with the invented claims circled.
- **Source/licence:** live demo — nothing embedded.

## Slide 13 — Good output starts with a good brief

- **On-slide text:** Most bad output is a prompt problem, not a model problem · Brief it like a fast, well-read new colleague with no context · Role · Task · Context · Format · Example.
- **Speaker notes:** Reframe prompting as briefing, not incantation. The model is capable but knows nothing about your specific situation, so you supply that. Walk the five parts once. Emphasise that the biggest lever, by far, is specificity plus giving it the actual source material to work from.
- **Visual:** ```mermaid
flowchart TD
    R["Role"] --> T["Task — one clear ask"] --> C["Context + source material"] --> F["Format"] --> E["Example (if shape matters)"]
```
- **Source/licence:** original.

## Slide 14 — Before and after

- **On-slide text:** Before: "Write release notes." → invents audience, format, and content · After: role + the 12 real PR titles + "group into Features/Fixes/Known Issues, one sentence each, invent nothing" · Same model. Completely different result.
- **Speaker notes:** The money slide for the prompting section. Read both aloud. The "after" wins on specificity, real source material (which makes the output checkable), and an explicit "don't invent" instruction. It quietly demonstrates the safety habits too. Then the honest anti-pattern: there is no magic phrase — "act as a world-class expert" does far less than one concrete example.
- **Visual:** Two-column before/after card. Optional third column: "why it's better".
- **Source/licence:** original.

## Slide 15 — Take away five things

- **On-slide text:** 1. Learns from examples, not rules · 2. Autocomplete on steroids — doesn't look things up · 3. Hallucination is structural; confidence ≠ truth · 4. Use where you can verify, or truth doesn't matter — own the output · 5. Good prompts are clear briefs, not magic words.
- **Speaker notes:** Recap the arc. Then the one sentence to leave in the room: an LLM is a fluent, confident, sometimes-wrong assistant — brilliant when you can check its work, dangerous when you can't. Treat it accordingly. Point to the full course for anyone who wants more.
- **Visual:** The five points as a numbered list; the one-sentence takeaway boxed at the bottom.
- **Source/licence:** original.

## Slide 16 — Questions, and where to go next

- **On-slide text:** Questions? · Full 16-session course · Start with: Session 1, Session 13 (confidently wrong), Sessions 10–11 (prompting) · Glossary for every term.
- **Speaker notes:** Open the floor. Common questions to expect: is our data safe to paste (depends — treat as external), will it replace jobs (see Session 15), which tool is best (they're similar; the skill transfers). Point to the glossary and the full series.
- **Visual:** Resources + contact layout.
- **Source/licence:** none.
