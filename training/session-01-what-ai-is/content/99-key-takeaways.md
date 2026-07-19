# Session 1 — Key Takeaways

A tight recap. If a slide, a colleague's question, or your own memory of this session ever conflicts with this file, this file wins.

## The four ideas

1. **Learning by example, not by rules.** Classical software is `data + rules → answers`; machine learning is `data + answers → rules`. We invert because for perception and language nobody can *write* the rules — a model *infers* them from labelled examples. The price: the learned rules are opaque numbers, and they inherit any skew in the examples.

2. **Human memory is reconstructive.** We rebuild the past from fragments plus expectations and experience the rebuild as playback. That manufactures *confident* false memories — and you cannot feel the difference between a true one and a false one. Certainty is not a truth signal.

3. **Hallucination, false memory, and prejudice are one failure mode.** All three are **pattern-completion outrunning the evidence**, with confidence tracking plausibility rather than truth. Prejudice is that mechanism pointed at people: an over-generalisation from skewed data. AI bias is hallucination with a demographic target — not a separate problem.

4. **An LLM is autocomplete on steroids.** It *generates* the next likely token; it does not *retrieve* facts. It is a **pattern-matcher, not a search engine**. Right answers ride strong patterns; hallucinations are the same machinery where patterns are thin.

## Precision vocabulary to keep

| Term | Meaning | Source |
|---|---|---|
| Model | The learned "rules," stored as numbers (weights) | file 01 |
| Reconstructive memory | Memory as rebuild-from-fragments, not playback | file 02 |
| Pattern-completion outrunning evidence | The shared mechanism of hallucination, false memory, prejudice | file 03 |
| Intrinsic hallucination | Output that **contradicts** the given source | Maynez et al. 2020 |
| Extrinsic hallucination | Output that **can't be verified** against the given source | Maynez et al. 2020 |

## The habit of mind

Don't ask *"is the AI lying?"* — it can't; lying needs a known truth to hide. Ask:

> **"Have I checked this pattern-completion against evidence, or am I trusting it because it sounds right?"**

That one question works on a model's output, on your own certain-feeling memory, and on a snap judgement about a person.

## If you remember one thing

> **An LLM is autocomplete on steroids — a pattern-matcher, not a search engine. Everything about its cost, its risks, and its limits follows from that one fact.**
