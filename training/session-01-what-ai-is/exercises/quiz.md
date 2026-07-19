# Quiz — Session 1

Eight self-check questions. Try them before looking at the answer key at the bottom. Aim to *explain*, not just recognise.

---

**Q1.** Fill in the two arrows. Classical software: `data + ______ → answers`. Machine learning: `data + ______ → rules`.

**Q2.** Give one example of a problem where *learning from examples* clearly beats *writing the rules*, and say in one sentence why writing the rules fails there.

**Q3.** True or false: "A false memory feels vaguer and less certain than a true one, so you can usually tell them apart." Justify your answer.

**Q4.** State the single failure mechanism that hallucination, confident false memory, and prejudice all share. Use the phrase from the session.

**Q5.** A recruiting model trained mostly on one demographic's past hires confidently down-ranks a strong candidate from outside that group. Is this a *different* problem from an LLM inventing a fake citation, or the *same* one? Explain.

**Q6.** Define **intrinsic** and **extrinsic** hallucination (Maynez et al. 2020), and say which one grounding/RAG mainly addresses.

**Q7.** Why is a *plausible* wrong answer more dangerous than an *obviously* wrong one? Connect your answer to why "put a human in the loop" is necessary but not sufficient.

**Q8.** Complete and explain the session's one-line mental model: "An LLM is autocomplete on steroids — a ______, not a ______."

**Bonus.** A colleague says: "LLMs and human brains hallucinate for exactly the same reason — they're the same mechanism." How should you respond, given the session's stance?

---

## Answer key

**A1.** `data + rules → answers` (classical); `data + answers → rules` (machine learning). The machine *infers* the rules from labelled examples instead of a human writing them. (file 01)

**A2.** Any perceptual/fuzzy task: e.g. **spam detection** or **cat-vs-dog in a photo**. Writing rules fails because the rules are too many, too fuzzy, and constantly shifting (spammers adapt; no finite list of features separates cats from dogs), yet examples are plentiful and a model can infer the boundary from them. (file 01)

**A3.** **False.** Reconstruction produces false memories that feel exactly as vivid and certain as true ones — the misinformation effect and DRM word-list studies show people reporting never-presented details with high confidence. *Certainty is not a truth signal.* (file 02)

**A4.** **Pattern-completion outrunning the evidence** — filling a gap with the most plausible pattern and reporting it with confidence that tracks plausibility, not truth, because nothing checks the completion against evidence. (file 03)

**A5.** The **same** problem. The recruiting model is hallucinating a conclusion about a person from skewed data — pattern-completion outrunning evidence, pointed at people. Prejudice is that mechanism aimed at individuals; the fake citation is it aimed at a fact. Same bug, same counter-habit (verify against evidence). The stakes and moral dimension differ, but the mechanism is one. (file 03)

**A6.** **Intrinsic** hallucination: output that **contradicts** the source content you provided. **Extrinsic** hallucination: output that **cannot be verified** from the source — it introduces content the source neither supports nor denies. **Grounding/RAG** mainly targets the *extrinsic* kind (it gives the model a source to answer from); it does **not** by itself cure intrinsic hallucination, since a grounded model can still contradict its source. (file 04, Maynez et al. 2020)

**A7.** A plausible falsehood pattern-matches to "correct," so it passes review and gets acted on; an obvious error is discarded harmlessly. That's why a human reviewer alone isn't enough — they're being asked to catch the one fluent item that looks exactly like all the other fluent items, and the better the system gets, the rarer and harder-to-spot the error becomes. (files 03–04; foreshadows Session 13)

**A8.** "A **pattern-matcher**, not a **search engine**." An LLM *generates* the next likely token rather than *retrieving* a stored fact; right answers ride strong patterns, hallucinations are the same machinery where patterns are thin. Cost, risk, and capability all follow from this. (file 04)

**Bonus.** Agree the **parallel is striking and useful**, but correct the overstatement: it is an **analogy, not a proven identical mechanism** — the source that popularised the comparison says so explicitly. The genuinely *shared* thing is the abstract failure pattern (pattern-completion outrunning evidence), not the biological and computational details, which differ. Naming that limit is part of the course's honest voice. (file 02)
