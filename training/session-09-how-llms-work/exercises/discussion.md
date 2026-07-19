# Discussion & Polls — Session 9

Prompts for the 15-minute Q&A block and two optional in-session polls. Each prompt states what a good answer surfaces, so the facilitator can steer without lecturing.

---

## In-session polls (60–90 seconds each, optional)

### Poll A — after the tokens segment (slide 6)

> **Which of these costs the most tokens?**
> **A)** 1,000 characters of English release notes
> **B)** 1,000 characters of German release notes
> **C)** 1,000 characters of a JSON build manifest
> **D)** 1,000 characters of SHA-256 hashes

**Answer: D**, by a wide margin (roughly 600–900 tokens vs. ~250 for English) — a hash has no frequent substrings, so BPE falls back to near-byte level. C is next, then B, then A.
**What it surfaces:** that token cost is a property of *text shape*, not text length — and that the artefacts this team works with daily (manifests, logs, hashes, part numbers) sit at the expensive end.

### Poll B — after the temperature segment (slide 16)

> **Your team sets `temperature = 0` for an incident-summarisation tool. What does that guarantee?**
> **A)** The output will be accurate
> **B)** The output will be reproducible
> **C)** Both
> **D)** Neither

**Answer: B — and only weakly.** Temperature 0 removes sampling randomness, so the *sampler* is deterministic. It does nothing for accuracy: you get the most probable fabrication, every time. And full reproducibility additionally requires a pinned model version and a logged, byte-identical prompt, because batched GPU arithmetic and silent model updates defeat temperature 0 on their own.
**What it surfaces:** the single most common and most expensive misconception about this knob. Expect a meaningful number of C votes — that's the teaching moment.

---

## Q&A prompts

### 1. (Seed) Given the mechanism, which of our AI use cases is the machine structurally unsuited for?

**What a good answer surfaces:** the ability to name a *stage*, not just a vibe. Strong answers sound like: "we ask it to quote exact part numbers, and numbers get shredded into arbitrary fragments before any reasoning happens"; or "we paste the entire incident history in, which is textbook context rot"; or "we're using it where nobody downstream can check the output, and verification is the missing component."
**Facilitator note:** push back gently on "it's unsuited for anything important" — that is not the lesson. The lesson is that suitability depends on whether the output is *verifiable*, which is a design property you control.

### 2. Attention weights show you where the model looked. Is that an explanation of what it did?

**What a good answer surfaces:** the routing-vs-reasoning distinction from `content/03`. Weights show where information moved, not why or what was concluded. Information also flows through residual connections and feed-forward layers that no attention map displays, and a 96-layer × 96-head model has thousands of maps that do not compose into a narrative.
**Why ask it:** interpretability theatre is a real procurement risk. A vendor showing you a pretty heat map and calling it explainability is making a claim the mechanism does not support. Contrast deliberately with Session 5's decision tree, which genuinely is readable.

### 3. A vendor is pitching us a model with a one-million-token context window. What do you ask them?

**What a good answer surfaces:** "supports 1M" ≠ "trained on 1M-token documents" ≠ "performs well at 1M." Good questions: what is the accuracy curve on *our* kind of task as length grows? Was it trained at that length or extended afterwards? What does the price per request look like at 200K tokens versus 8K? What is time-to-first-token at full length? Can we measure degradation ourselves on our own documents before committing?
**Facilitator note:** this is the session's most directly job-relevant prompt for the managers in the room. Let it run.

### 4. Why can't the model just tell us when it doesn't know?

**What a good answer surfaces:** confidence is the sharpness of a probability distribution, and sharpness tracks *pattern strength*, not evidence. A fabricated citation follows a very strong format pattern and can therefore be produced with a sharper distribution than a genuine but rarely-stated fact. There is no stage that could compare the output to the world.
**The honest extension, if someone raises it:** calibration research and token-level confidence signals do exist and can help somewhat, and models can be trained to say "I don't know" more often. But these are *estimates layered on top*, tuned by post-training, not a truth check inside the mechanism. Do not let the room leave believing the problem is solved.

### 5. If it commits to token 1 before token 40 exists, how does it ever produce a coherent long answer?

**What a good answer surfaces:** each token conditions on everything before it, so coherence is maintained by consistency-with-context rather than by a plan. That is genuinely powerful and also genuinely fragile — a wrong opening sentence becomes context the model must stay consistent with. It also explains why chain-of-thought prompting works: it converts thinking into tokens the later tokens can attend to.
**Follow-up worth asking:** "So what should you do when the first sentence of an answer is wrong?" (Restart the turn. Do not argue it into a correction inside the same completion — the error is already in the context.)

### 6. Where in this pipeline would you put a check, if you were designing the system around it?

**What a good answer surfaces:** the realisation that every viable check is *outside* the model. Run the generated code. Resolve the DOI. Query the part number against the real database. Diff the summary against the source. Require a trained human to sign. Constrain the task so the output is checkable at all. This is the direct on-ramp to Sessions 13 and 13.
**Facilitator note:** if the room proposes "ask the model to check its own output," take it seriously and then push: it helps somewhat, and it is the same mechanism with no new information, so it cannot be the last line of defence.

### 7. Which is worse for us — a model that is wrong 20% of the time, or one that is wrong 1% of the time?

**What a good answer surfaces:** the verification paradox. The 1% model is harder to supervise, because vigilance decays when it is almost never rewarded, and the rarer the error the more output you must check to find it. The 20% model keeps the human sceptical. This is a Session 14 argument, planted here because the mechanism has just made it credible.
**Facilitator note:** a deliberately uncomfortable question. Do not resolve it — leave it running into Session 14.

### 8. Session 1 said "autocomplete on steroids." Having seen the machine, is that fair or is it a cheap shot?

**What a good answer surfaces:** both halves. It is *mechanically* accurate — the objective genuinely is next-token probability and there genuinely is no verification stage. It is also *rhetorically* incomplete, because "autocomplete" undersells what attention over 96 layers of learned representation actually accomplishes, and dismissiveness is as unearned as hype.
**Facilitator note:** the right landing is that the mechanism is simple to state and its behaviour is not trivially predictable from that statement. Whether the system "understands" anything is Session 16's question, not this one. Holding both is the course's voice.
