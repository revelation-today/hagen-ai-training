# Quiz — Session 16

Ten self-check questions. Answers at the bottom. Cover the answers first — the point is to find out what did *not* land.

---

### 1. Multiple choice
Which of the seven AGI definitions in the timeline describes a property of **the economy** rather than a property of **the system**?

- **A)** Turing (1950) — behavioural imitation
- **B)** Chollet (2019) — skill-acquisition efficiency
- **C)** The 2023 industry definition — "economically valuable work"
- **D)** ARC-AGI (2025) — generalisation beyond training data

### 2. Short answer
A colleague says *"AGI will be here in three years."* Give the **two** questions you should ask before agreeing or disagreeing.

### 3. Multiple choice
Of the seven pillars of intelligence, which is **most clearly absent** in a deployed LLM?

- **A)** Language
- **B)** Memory
- **C)** Learning
- **D)** Reasoning

### 4. True or false — and explain
*"A newer, more capable model will always be at least as good as the model it replaces."*

### 5. Short answer
The chess world-model probe found that the model identified **whose turn it was 100% of the time**. Why is this result much less impressive than it sounds, and which result from that experiment is the meaningful one?

### 6. Short answer
What is the current status of **ARC-AGI-1** versus **ARC-AGI-2**, and why does the way ARC-AGI-1 was solved partially undercut the achievement?

### 7. Multiple choice
Which of the five Transformer limits is described as **the deepest** — the one least likely to be fixed by scaling?

- **A)** O(n²) context scaling
- **B)** Shallow vectorised reasoning
- **C)** No embodiment / no grounded world model
- **D)** Intellectual monoculture

### 8. Short answer
Correct this statement: *"A quantum computer holds all possible answers at once through superposition, so it tries every possibility in parallel and picks the right one."*

### 9. Multiple choice
A press release announces a **1,000-qubit quantum processor**. What is the single most important clarifying question?

- **A)** What is the operating temperature?
- **B)** Are those physical or logical qubits?
- **C)** How many gates can it apply per second?
- **D)** Which company manufactured the chip?

### 10. Short answer — the applied one
Your team ships an embedded product with an expected 12-year service life. Name **two** concrete quantum-related actions worth taking this year, and one that is **not** worth taking.

---
---

## Answer key

**1 — C.** The 2023 industry definition ("broadly smarter than humans at economically valuable work"). Every other definition on the timeline describes a property of the system — does it reason, does it generalise, does it acquire skills efficiently. The economic definition describes labour-market substitution, which means it can move when *prices* move rather than when capability moves. It can be satisfied by a narrow tool that is merely cheaper than a human, and unsatisfied by a genuinely general system that is too expensive to deploy. It is a business milestone wearing a scientific term. (`content/01` §1.3)

**2 —** (a) **Which definition of AGI are you using?** and (b) **What measurement would settle it?** If the definition is unstated, the conversation usually resolves right there. If no measurement exists, the claim is a belief rather than a forecast — which may be sincere but is not something to plan around. Two good bonus questions: *who benefits if this is believed*, and *what would we observe if it were false*. (`content/01` §1.5)

**3 — C, Learning.** A deployed model does not learn. Training halts, weights freeze, and the model ships with a knowledge cutoff it can recite. Everything that resembles post-deployment learning is one of three other things: text placed back into the context window, an external store being written to, or a separate and expensive retraining run. This matters because the ability to acquire new skills on the fly is arguably the defining feature of the word *general*. (Memory is also weak, but it is *partially* addressed by RAG and external stores — learning is not addressed at all.) (`content/02` §2.3, §2.5)

**4 — False.** Capability is not a scalar. Reasoning models (o3, o4-mini-class) showed **higher** hallucination rates than the earlier non-reasoning 4o on factual-recall evaluations `[verify at delivery]`. A model optimised for multi-step derivation can regress on single-step recall. The operational consequence: **a model upgrade is a dependency change and requires regression testing.** If you have a working prompt or pipeline on model N, re-run your evaluation set on N+1 before switching. Most teams skip this, and most teams do not have an evaluation set to run — which is the real problem. (`content/03` §3.4)

**5 —** Whose turn it is, is simply the **parity of the number of moves in the list** — a trivial count, requiring no board representation at all. The meaningful result is the other one: **absolute piece positions were only ~10–11 percentage points better than random guessing.** A genuine internal board representation would score near-perfectly, because chess state is fully determined by the move list with no ambiguity or hidden information. A ten-point margin is consistent with "learned which pieces tend to have moved by a given point in a game", not with tracking a board. Scope it honestly though: one probe, one model, one domain. (`content/03` §3.3)

**6 —** **ARC-AGI-1 is effectively solved** (~75% by an o3-class system) `[verify]`; **ARC-AGI-2 remains open — humans solve these tasks, AI does not** `[verify]`. The undercut: the high ARC-AGI-1 scores came at compute costs per task orders of magnitude above what a human needs. Since the benchmark exists to measure *skill-acquisition efficiency*, brute-forcing it with enormous search is a partial answer at best — it demonstrates that the tasks are solvable, not that they were solved efficiently. Goertzel's 2007 phrase, *"using limited resources"*, was doing real work. Be fair, though: a benchmark explicitly designed to be memorisation-proof was substantially beaten, and that is a real achievement. (`content/03` §3.5)

**7 — C, no embodiment / no grounded world model.** Text-trained Transformers learn the statistics of *descriptions* of the world, not of the world. The intuition: you knew a thrown ball comes back down before you could speak in full sentences, from watching things fall — not from reading about gravity. A system trained only on text has the sentences without the intuition beneath them. The chess probe is the empirical support. B (shallow vectorised reasoning) is also considered unfixable by scaling and is a defensible second answer; A is partially addressable by approximations, and D is sociological rather than architectural. (`content/05` §5.5, §5.7)

**8 —** The correction has two parts. **(a)** Superposition is not "all answers at once" in a way you can read — an *n*-qubit register spans 2ⁿ amplitudes, but **measurement yields exactly one *n*-bit classical string**, and the rest is destroyed. **(b)** The computational power comes from **interference**: a quantum algorithm arranges the amplitudes so that wrong answers cancel and the right answer reinforces, making the single measurement likely to return what you want. It is not a parallel search — it is amplitude engineering, and only a short list of problems has a known structure that permits it. (`content/06` §6.2)

**9 — B, physical or logical.** Error correction, not qubit count, is the bottleneck. Because qubits cannot be copied (no-cloning), classical redundancy is unavailable, so one **logical** (usable, error-corrected) qubit requires on the order of 10²–10³+ **physical** qubits `[verify]`. A thousand noisy physical qubits and a hundred good logical qubits are not comparable objects, and press releases usually do not distinguish them. Two good follow-ups: what is the two-qubit gate error rate, and what circuit depth can it sustain? (`content/06` §6.4)

**10 —** **Worth doing this year:** (1) a **crypto-agility assessment** — can the product's cryptographic algorithms be replaced in the field, or are they fixed at manufacture? (2) a **key and certificate inventory** — you cannot migrate what you have not catalogued, and this is a configuration-management task before it is a cryptography task. Other good answers: plan a hybrid classical+post-quantum rollout; check whether third-party components and HSMs have a PQC roadmap; confirm firmware and code-signing signatures will remain verifiable across the transition.
**Not worth doing:** investing in **quantum machine learning** or any "quantum AI" capability. There is no demonstrated quantum advantage on a practically useful ML task, and there are structural obstacles — data-loading cost and barren plateaus — with no accepted solution. A 12-year service life makes the *cryptographic* exposure real; it does not make the AI intersection any less speculative. (`content/06` §6.5, §6.6)

---

### Scoring, loosely

| Score | Read |
|---|---|
| 9–10 | You can run the four-question filter on the next horizon claim unaided. That was the goal. |
| 6–8 | Solid. Re-read `content/03` (the evidence) and `content/06` §6.4 (error correction). |
| ≤5 | Re-read `99-key-takeaways.md`, then the two files above. The evidence section is the load-bearing part. |
