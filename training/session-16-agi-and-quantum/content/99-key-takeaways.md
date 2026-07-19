# 99 — Key Takeaways

Session 16 · What Is AGI, and an Outlook on Quantum · **the series closer**

---

## Part A — AGI

**The definition**
- **AGI has been redefined roughly every decade since 1950** — Turing (imitation) → Newell & Simon (symbols) → Legg & Hutter (goals across environments) → Goertzel (…with limited resources) → Chollet (skill-acquisition efficiency) → industry (economically valuable work) → ARC-AGI (generalisation beyond training data).
- The 2023 industry definition is the odd one out: it describes a property of **the economy**, not of **the system**. It is a business milestone in scientific clothing.
- Before debating "is AGI close?", ask **which definition** and **what measurement would settle it**. Most of the time the conversation resolves right there.

**The seven pillars — a checklist for what's missing**
- Reasoning · Memory · Learning · Language · Perception · Self-awareness · Motivation/values.
- **Strongest: language.** Which is precisely why it misleads — fluency is what humans instinctively read as general intelligence.
- **Weakest: learning.** Deployed models do not learn. Weights are frozen; everything that looks like post-deployment learning is context, an external store, or a separate retraining run. This is the gap most central to the word *general*.
- Real capabilities are **composites** (planning ≈ reasoning + motivation; reflection ≈ + self-awareness). Benchmarks test single pillars. That is a core reason scores and general capability come apart.

**The evidence** `[verify all at delivery]`
| Result | What it shows |
|---|---|
| **Tower of Hanoi** (Apple, *The Illusion of Thinking*) | Reasoning models **collapse** rather than degrade past a complexity threshold — and sometimes reduce effort as difficulty rises. Correlation ≠ algorithm. |
| **Chess world-model probe** | Turn tracked 100% (trivial); piece positions only **~10 pts better than random**. No usable world model emerges from text alone. |
| **o3 / o4-mini vs. 4o hallucination rates** | Reasoning models hallucinated **more** on factual recall. **Capability is not a scalar — model upgrades are not strictly improvements.** |
| **ARC-AGI-2** | **Human-solvable, AI-unsolved.** The cleanest single counter-example to "general". (ARC-AGI-1 was solved ~75% — at extreme compute cost, which undercuts the *efficiency* the benchmark measures.) |

**The labs disagree — and that is the finding**
- **Anthropic (Amodei):** AGI is a transition, not a moment.
- **Meta (LeCun):** today's models are smart parrots; embodiment and world models are required.
- **Mistral (Mensch):** the pursuit is quasi-religious; the concept is a category error.
- **DeepSeek:** progress is efficiency, not scale.
- **AI2 (Etzioni):** you cannot evaluate AGI with a single number.
- **OpenAI (Altman):** economically valuable work, sooner than most think.
- Every position aligns with its holder's strategy. **Weight demonstrations over declarations.**

**Five structural limits of the Transformer**
1. **O(n²) scaling** — context costs quadratically; a bigger buffer is not memory.
2. **Shallow vectorised reasoning** — CoT is generated text, not verified state; no unbounded iteration.
3. **Architectural rigidity** — no persistent state, no modular planning, systematic positional bias.
4. **No embodiment / world model** — statistics of descriptions, not of the world. *The deepest one.*
5. **Intellectual monoculture** — scaling one architecture starves the alternatives, self-confirmingly.

Limits 2 and 4 are **not obviously fixed by scaling.** The field's response — new architectures and hybrids — is itself an admission of that.

---

## Part B — Quantum ⚠️ *the most speculative content in the series*

- **A quantum computer is not a faster computer.** It is a different computer: dramatically faster on a short list of problems, worse at almost everything else. It will never speed up your build.
- **Superposition + entanglement + interference.** The power lives in **interference** — arranging amplitudes so wrong answers cancel. You measure once and get **one classical string**; there is no free parallel search.
- **Error correction, not qubit count, is the bottleneck.** One logical qubit costs ~10²–10³+ physical qubits `[verify at delivery]`. When you read "1,000 qubits", ask: **physical or logical? gate error rate? circuit depth?**
- **Timeline: eras, not dates.** NISQ (now) → early fault tolerance → useful fault tolerance → cryptographically relevant. Anyone giving you a year is guessing.
- **The AI intersection is early research.** No demonstrated quantum advantage on a practically useful ML task. Two structural obstacles: **data loading** can cost more than the speedup buys, and **barren plateaus** make training hard. Several proposed QML speedups have been **dequantised**.
- **The one near-term item that is real: post-quantum cryptography.** Shor's algorithm breaks RSA/DH/ECC (symmetric crypto is fine with longer keys). **"Harvest now, decrypt later"** makes it urgent *before* the machine exists. NIST standards are published (public domain, slide-safe).
- **PQC is a configuration-management problem:** crypto-agility, key and certificate inventory, firmware/code signing, hybrid rollout, supply chain, long-lived device fleets.

> **Straight answer for this room:** near-term impact on your work is minimal; the AI intersection is speculative today; **anyone selling "quantum AI" now is selling futures.** The exception is crypto-agility — and that one lands on your desk.

---

## Closing the series

- Session 1's model — **autocomplete on steroids: pattern completion, not lookup** — correctly predicts Session 16's frontier evidence. Good mental models keep paying.
- The five habits worth keeping: *what is it generating* · *the metric can hide the failure* · *constrain the system to do less* · *verify, and check the verifier* · *who made this claim and what would falsify it*.
- The portable skill is the **four-question filter**. It worked on AGI. It worked on quantum. It will work on the next thing, which is not named yet.

---

## If you remember one thing

> **There exists, today, a set of puzzles that ordinary humans solve and frontier AI does not — and the labs building that AI cannot agree on whether "AGI" even means anything. You don't need to predict the future. You need to be able to evaluate the person who claims they can.**
