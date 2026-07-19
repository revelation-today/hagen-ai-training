# Session 2 — Key Takeaways

## The vocabulary

- **AI ⊃ ML ⊃ Deep Learning ⊃ LLMs** — strict containment. The interesting systems are often in the *dotted branches*: rule-based AI (no learning at all), classical ML (trees, forests, clustering), non-language deep learning (vision).
- **Descending the stack** buys generality and tolerance of messy input; it costs auditability, determinism, and money. Descend only as far as the problem forces you.
- **"Deep" has no defined layer threshold.** It is a convention, not a definition. Say so when asked.
- A **model** is fitted numbers plus the arithmetic that applies them. Nobody wrote the rule; it was fitted.
- **Training** finds the numbers (rare, expensive, needs labels). **Inference** uses them (constant, cheap per call). When you call a hosted LLM you pay for **inference only** — the training bill was the vendor's.
- **Parameters** are learned by the machine; **hyperparameters** are chosen by a human before training. Parameter count tells you about *hardware*, not about *quality*.
- Unrecorded hyperparameters make a model **irreproducible**. That is a configuration-management defect, and it belongs in your discipline, not data science's.
- An LLM is the only layer that **never trained on your data**. You supply it per call — which moves your data volume off a one-off training bill and onto a recurring per-token one.

## The token

- A token is a **subword chunk** from a learned vocabulary. Your text becomes a list of integers; the length of that list is your bill.
- **1 token ≈ ¾ of an English word ≈ 4 characters.** 1 word ≈ 1.3 tokens. 1,000 words ≈ 1,300 tokens.
- **German, code, JSON, and log lines cost 1.5–4× more tokens per word** than English prose. Serialisation format is a free cost lever — a markdown table beats JSON for the same content.
- **Output tokens are expensive because each one is a full pass through the model.** That is the mechanism behind the ~5× price.
- Estimate with the rule of thumb; **bill from the API's reported usage.** Instrument token counts on day one of any pilot — prices change, token counts are the stable measurement.

## The cost

- Input and output are **separate line items**. Output is typically **4–5× more per token**.
- In a realistic triage workload, output is **~18 % of the tokens and ~52 % of the cost.** "Be concise" is a cost control.
- The **tier spread is ~60×** for the same task. Choosing the tier is a bigger decision than any prompt optimisation — and it should be settled by testing on real inputs, not by assumption.
- The **context window** is a hard ceiling *and* a cost multiplier. The API is stateless; the illusion of memory is produced by re-sending the whole transcript every turn.
- Conversation cost grows **quadratically**: 20 turns costs ~3× what a per-request estimate predicts, and ~9× on input tokens alone. The last turn of a long chat costs ~17× the first.
- **Three multipliers act without changing your request count:** long documents (linear, and they persist across turns), RAG (a constant added by a config file nobody reviews for cost), and agents (**one user request → 8+ billed calls → ~13× cost**).
- **Levers in order of leverage:** change tier (60×) → cache the stable prefix (~90 % off repeated input) → cap output (5× unit price) → trim input → batch (50 %) → don't call the model at all.
- **Put stable content first, variable content last.** A reordering, not a rewrite, and it can remove ~80 % of a document-heavy workload's cost. A timestamp at the top of your prompt costs you the entire cache.
- At small volume, inference is often ~1 % of the labour it displaces — so **don't over-optimise early**. But know exactly which multipliers invert that, because they act fast and silently.

## Governance points for this audience

| Situation | The question that actually matters |
|---|---|
| Sizing a pilot | How many **tokens** per call, and what's in the context? |
| Reading a vendor quote | Per-token in *and* out — and what does the vendor silently add to the context? |
| Investigating a cost spike | Did the **context** grow? A document, more history, a new tool, a bigger `k`? |
| Reviewing a design change | Does it add **tokens per request**? Adding retrieval adds zero requests and 20,000 tokens. |
| Reviewing an agentic feature | How many **model calls per user action**? An agentic change is a cost change. |

## If you remember one thing

> **Cost scales with tokens, not with requests.**
>
> Two systems handling identical request volumes can differ by 10× on the invoice. Every billing instinct you have from transactions, tickets, seats, and API calls is measuring the wrong quantity here — and it will be wrong in the expensive direction, because contexts only ever grow.
