# Discussion Prompts — Session 12

For the 15-minute Q&A block, plus two in-session polls. Seven prompts; you will use three or four. Each notes what a good answer surfaces, so the facilitator can steer rather than just collect opinions.

---

## In-session polls

### Poll A (slide 3, minute ~2) — "Which of these is an agent?"

Three product descriptions: (a) a ticket triage pipeline; (b) a chat assistant that looks up an order status; (c) a system given "find out why component X regressed" plus eight tools that runs until it decides it is done.

**Options:** all three · only (c) · (b) and (c) · none of them.

*What it surfaces:* the room will split roughly evenly, which is the point — the word is used for at least four different things. **Do not resolve it.** Say you will come back at slide 7. The lasting value is the moment people notice they were using the same word for different systems in the same meeting.

### Poll B (after slide 15) — "Did the architecture win, or did the token spend win?"

**Options:** the architecture · the token spend · genuinely cannot tell from what we've been shown.

*What it surfaces:* the third option is the correct one and it is usually the least chosen, because "cannot tell" feels like a non-answer. Make the case that it is the most defensible position available — the lab's own variance decomposition points strongly toward token spend, but it does not eliminate an architectural contribution. Being able to hold "the evidence does not settle this" without discomfort is the actual skill.

---

## Q&A prompts

### 1. "Name a task in your area someone has proposed automating with an agent. Walk it through the four gates."

*What a good answer surfaces:* the four gates being used as an instrument rather than recited. The best answers get stuck at gate 2 (enumerability) and discover, out loud, that the proposal is a workflow. The second-best get to gate 4 and discover that the errors are not recoverable, which is the moment the design changes from "an agent" to "an agent that proposes and a human that approves." A weak answer waves at gate 3 ("the model is good at this now") without a number — push back: *measured how, on how many real tasks?*

### 2. "Where do we already have an API acting on model output — even without calling it an agent?"

*What a good answer surfaces:* the uncomfortable inventory. Most teams find at least one: an auto-labelling job that writes to a ticket system, a summariser whose output is pasted into a field that downstream tooling parses, a script that classifies a log line and opens a defect. None of these were built as agents; all of them are the hazard from slide 21. This prompt produces the most useful silence in the session. Follow-up: *who reviews it, and when did they last actually read one?*

### 3. "Our vendor's agent scores 88% on an industry benchmark. What do you ask them?"

*What a good answer surfaces:* the four questions from `content/06` §5. Strong answers get to "cost per task" quickly. The best answers also get to *"same harness?"* — that agent scores swing with the scaffold, not just the model, so a comparison across different harnesses is not a comparison of models. If the room only produces "what's the eval set?", supply the pass^k point: 88% pass^1 commonly means low-to-mid 70s when the same task is retried across sessions, and production is a retry environment.

### 4. "An agent proposes a configuration change with cited evidence. Who is qualified to approve it, and how do we stop them from rubber-stamping?"

*What a good answer surfaces:* that "human-in-the-loop" is a design problem, not a checkbox. Good answers name a *specific role* with the competence to evaluate that specific output, and then confront the harder half: an approver who sees a stream of correct proposals stops reading. Look for concrete countermeasures — small diffs rather than whole files, evidence cited by identifier so it can be spot-checked, a mandatory "what was not verified" section, a default of reject when the trace is unclear, and periodic deliberate injection of known-bad proposals to measure whether the gate is actually functioning. Connect forward to Session 13: the better the system gets, the harder its rare errors are to catch.

### 5. "We have budget for one agent this year. Read-only investigator, or one that opens the fix?"

*What a good answer surfaces:* an argued trade-off rather than a preference. The read-only case: an entire failure class removed at almost no cost to the learning, and you get real measurements on real tasks. The acting case: the value is concentrated in the acting, and errors *are* cheap when the action is a pull request behind review, tests, and CI. The right answer is usually read-only first — but the person who argues for the PR agent and correctly identifies that its viability rests entirely on existing review machinery has understood gate 4 better than someone who just picks the safe option.

### 6. "The step cap fires on 40% of our runs. What does that tell us?"

*What a good answer surfaces:* that a firing cap is information. Three readings, and a good answer names more than one: (a) the cap is too low for a genuinely long task; (b) the tools are wrong — the agent is flailing because it lacks the tool it needs, or two tools overlap and it keeps thrashing; (c) **the task was never agentic**, and the model is wandering because there is no data-dependent structure to discover. The diagnostic is the trace: look at whether the 40% are all long-and-productive or all repetitive. If a tool repeats with identical arguments, it is (b) or (c), not (a) — and raising the cap makes the bill worse without making the answers better.

### 7. "Is any of this settled enough to bet on? Argue the other side."

*What a good answer surfaces:* a genuinely open argument, and the room should be allowed to have it. The honest position has three parts, and a good answer holds all three at once: (a) capability has improved enormously and continues to — the trajectory is real, not marketing; (b) **reliability still lags capability**, and multi-step compounding means the gap matters more here than anywhere else in the course; (c) the *engineering* — bound, trace, cost, test, gate — is durable regardless of which way the capability question resolves, because it is ordinary systems discipline applied to a stochastic component. Do not let the room settle into either "it's all hype" or "it's inevitable." Both are lazier than the evidence supports.

---

## Facilitator notes

- **Prompt 2 is the highest-value prompt in this list.** If time allows only one, use it. It converts an abstract session into an inventory of things the team already owns.
- Prompt 4 sets up Session 13, prompt 2 sets up Session 14. Say so explicitly as you close.
- If someone in the room has already built an agent, hand them prompt 6 and let them talk. Real trace data beats any prepared example.
- Expect the question *"which framework should we use?"* — it always comes. The prepared answer: learn the loop first, treat frameworks as replaceable, read `smolagents` because you can read all of it, and note that the major frameworks genuinely disagree about the right abstraction, which is a good reason not to marry one yet.
