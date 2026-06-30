# LLM Magic Words: Second- & Third-Order Prompt Operators
### A Practitioner's Guide to Programming Language Model Behavior

*Draft v7 — Saved 2026-06-30*

---

## Part 0 — The Punchline

Two engineers sit at a café table. One says, "Some day we won't need coders. We'll just write the specification and the program will write itself." The other agrees: "Exactly — write a comprehensive, precise spec and bam." The first engineer asks: "Do you know the industry term for a project specification that's comprehensive and precise enough to generate a program?" Code. It's called code.

That punchline contains the entire thesis of this guide. Precision cannot be separated from programming. If you want reliable behavior from a language model, you must specify that behavior precisely enough to be executed reliably. In the LLM era, that specification is often written in plain English — but the work is still the work, and the rest of this guide is about three ways of seeing that fact: through the old idea of magic words, through a newer idea from radio engineering, and through what both of those mean for the surface you're actually writing on.

In an LLM, certain words and phrases reliably induce specific behavior — not as a style choice, but as a control signal the model treats differently from ordinary language. Some force a fixed output shape and refuse to drift from it. Some shut off the model's tendency to fill a gap with a plausible guess, and make it say "unknown" instead. Some name a failure mode explicitly and prohibit it, which changes how the model behaves around that failure for the rest of the session. Different words, different effects — but all of them convert a vague intention into an executable constraint. Used well, these words can seem to have almost magical properties.

Every culture that ever told stories about magic understood one thing: the words had to be spoken precisely. The wrong word, the wrong order, the wrong omission — and the spell did not work. Not because magic is arbitrary, but because precision is what separates intention from result. What's easy to miss is what the wizard actually *is* in those stories — not someone with access to power other people lack, but someone who has learned the grammar that makes the power reliable instead of accidental. Anyone can speak the same words a wizard speaks. Most people get nothing, because they're saying roughly the right thing in roughly the right order, and "roughly" is exactly what magic doesn't forgive. The wizard isn't talented. The wizard is precise, on purpose, every time. That's the whole difference, and it's learnable — which is the premise of everything that follows.

Wizardry is one way to see this. Radio engineering offers another, more literal one. For most of the twentieth century, a radio's behavior was fixed in its hardware — its modulation, its frequency range, its protocol, all baked into physical circuits. To make it do something different, you built different hardware. Then engineers found a way to move that logic into software: the same physical antenna and signal converter could become an AM receiver, an FM receiver, or a digital radio entirely, depending only on the software run against it. They called it software-defined radio. The hardware stayed generic and capable. The software defined what it actually became.

Language-defined programming is the same idea applied to AI. The language model is the generic hardware — vast capability, no fixed purpose. Your prompt is the software that defines what it becomes for this particular task: a state machine, a compliance monitor, a planning agent, whatever you need. The model doesn't change. What changes is the precision of what you define against it. What it lacks, on its own, is orientation: what to do, in what order, under what constraints, and what to do when something is missing. That orientation is what your prompt provides.

Here's the part worth sitting with: plain English, used this way, is a programming surface — and it's still a largely unmapped one. Traditional code has fifty years of accumulated discipline behind it: design patterns, type systems, established failure modes with names everyone agrees on. The control layer you write in English has almost none of that yet. Most of this surface is still being built one-off, one plausible-sounding sentence at a time, with no shared vocabulary for what's actually happening when it works and when it doesn't. The vocabulary in this guide is an attempt at exactly that: not incantations, but the first real grammar for a surface that's wide open and largely unexplored.

Here is the part that surprises people: this guide can write your prompt programs for you. Not as a metaphor — literally. Because everything in this guide is itself precise, structured language, a language model can read it the same way you do, and use it as a specification to draft a new governed program for whatever you describe. You will not just be learning to write prompt programs by hand. By the end, you will be handing this document to a model and asking it to write one for you — using the same vocabulary, correctly, on the first attempt. Part 8 walks through exactly how.

What follows is a guide to using that grammar well.

---

## Part 1 — What Kind of Thing Is an LLM, and What Is a Prompt Program

To get the most from a language model, it helps to understand what it actually is — not in a technical sense, but in a practical one.

When most people start using a language model for the first time, they approach it the way they might approach a very smart colleague who has read everything. They ask a question. They get an answer. They ask another. This works well and feels almost effortless — and for many everyday tasks, it is exactly the right way to start.

But over time, something shifts. The same request asked twice produces different answers. A long, detailed task comes back slightly wrong in ways that are hard to pin down. The model fills in details with apparent confidence — details that turn out to have been invented. The outputs look right. They just are not always *right*.

This is not a flaw in the model. It is a feature of what the model fundamentally is.

Think of it this way. When you walk into a bakery and say "give me something good," the baker uses their judgment to choose for you. On a good day, it is perfect. On a different day, with different information about your preferences, you might get something you did not want. The result depends entirely on how clearly you expressed what you needed — and how much the baker already knows about you.

A language model works the same way, at enormous scale and speed. It is not retrieving a stored answer the way a search engine finds a webpage. It is *generating* a response — building it from scratch, guided by what you gave it and by everything it absorbed during its training. When you give it precise direction, it follows that direction. When you leave gaps, it fills them — with something plausible. Plausible is not the same as accurate.

One more thing worth knowing about that training: it happened at a point in time. A language model is like a very well-read new employee who joined the organization two years ago and has not read anything published since. They know a great deal — but they do not know about the policy change from last month, the new competitor that launched this quarter, or the regulation that took effect last week. By default, a language model draws only on what was in its training and what you put in front of it during your conversation. When you need current information — recent events, live prices, updated regulations, real-time availability — you have to tell it to go look. Part 5 covers the operators for this.

The practitioner's shift is this: from *asking* to *specifying*. From describing what you want conversationally to defining what you need precisely. A language model is less like a search engine and more like a talented, eager new employee on their first week — skilled, willing, and in genuine need of clear direction. The clearer the direction, the more reliably the work gets done.

**What is a prompt program?**

A conversation with a language model is a back-and-forth — you ask, it answers, you refine. A prompt program is something different. When you write a set of instructions that a language model will follow consistently across different inputs, different users, and different sessions — you have written a prompt program. It is not a conversation. It is a specification.

Think of a paper form. A blank form gives every person who fills it out the same fields, the same sequence, the same constraints. It does not matter who fills it out or when — the structure holds. A prompt program does the same thing for a language model interaction. It defines the structure, the rules, the order, the edge cases, and the output — so that anyone who runs it gets a governed, consistent result.

The magic words in this guide are the building blocks of prompt programs. They are the vocabulary of clear specification — the difference between "give me something good" and "here is exactly what I need, in this order, with these constraints, and here is what to do if something is missing."

**Here is how this guide is organized.**

Part 2 introduces the idea that the words in this guide work at different levels — some do one thing directly, others govern how groups of words interact, and others set the posture for the entire program. Understanding those levels is what turns a collection of useful phrases into something that holds together reliably.

Part 3 takes that idea and grounds it in three problems every practitioner eventually runs into. Each problem has a solution — and together, those solutions form the foundation of a well-built prompt. The section builds a single working example across all three layers so you can see how they fit together.

Part 4 covers a fourth group of words that matters specifically when someone will scrutinize your output and ask "how do you know that?" — the Show Your Work operators.

Part 5 is the full toolkit across all four layers — the complete set of words organized by what they do, including operators for reaching beyond the model's training when current information is required.

Part 6 introduces the six types of prompt programs — each with its own shape, its own failure mode, and its own emphasis. Knowing the type before you write changes what you pay attention to.

Part 7 brings it together as a practice: a repeatable sequence for building a prompt program that holds, plus a blank canvas for starting from scratch.

Part 8 closes with the advanced capability this whole guide is building toward: using the guide itself as a compass and a french curve to write prompt programs automatically.

Part 9 closes the loop — what you now have, and where this takes you.

The appendices collect everything for quick reference.

---

## Part 2 — Learning to See the Levels

There is a moment in tenth-grade geometry that frustrates nearly everyone. The teacher hands out a proof problem — a set of shapes, a set of given facts, and a conclusion to reach — and asks the class to demonstrate, step by step, that the conclusion must be true. Not that it probably is. Not that it looks like it is. That it *must* be, given what is known.

Most students find this maddening. The answer seems obvious. Why prove it?

The answer to that question is the same answer this guide is pointing toward. Obvious is not reliable. Something that looks right can be wrong in ways that only appear under pressure — when the inputs are unusual, when the stakes are high, when someone else needs to verify the work. The proof is not for the obvious case. It is for every case. That discipline — if this is true, and this is true, then this must follow — is exactly what this guide is asking you to build into your prompts.

The magic words in this guide work at three different levels. You do not need to memorize the levels. You need to *feel* the difference between them, because the difference is what makes a prompt hold together rather than fall apart when something unexpected happens.

**First-order words** do one thing directly. They constrain a single behavior. `MUST`, `NEVER`, `ONLY`, `FOR EACH` — each one is a direct instruction, a single rule applied to a single thing. These are the vocabulary. Alone they take you a long way.

**Second-order words** govern how the first-order words work together. They do not constrain a single behavior — they constrain the *space between* behaviors. `GUARDRAIL (CRITICAL)` names a failure and forbids it. `EXECUTE IN ORDER` prevents two correct instructions from happening in the wrong sequence. `PREFERRED / FALLBACK / LAST RESORT` resolves what happens when two instructions could both apply. These words are the load-bearing connections — the engineering above the materials.

**Third-order words** set the posture of the entire program. They answer questions the specific instructions cannot: What does this program do when it runs out of information? What is the error behavior? `ERROR-MODE: FAIL-SAFE` is not an instruction about a specific output — it is a declaration about how the entire program behaves at its edges.

A prompt built only from first-order words is like a list of rules with no governing principle. When two rules conflict, the model chooses — and you did not get to make that choice. When information is missing, the model fills the gap — and you did not specify what to do there. Second- and third-order words are how you take those choices back.

Beyond the words themselves, every well-built prompt also gives the model three things that are easy to overlook: a sense of *how it should carry itself* — careful and precise, questioning rather than assuming; a clear sense of *what matters most* — what to focus on when there is too much to cover; and an awareness of *whether it is still on course* — so it does not drift toward generic helpfulness as the session grows longer. These are not separate instructions you add one at a time. They emerge from how well the program is written overall. Part 3 shows exactly how.

---

## Part 3 — Three Problems, One Foundation

Every practitioner who uses a language model long enough for high-integrity work eventually runs into three specific problems. They tend to appear in this order.

**The first problem: the output had the wrong shape.**

You asked for a summary with five sections. The model gave you four, combined two into one, and added one you did not ask for. The model was not being difficult. It was doing what it does — filling in what seemed right. The problem was that no one locked the shape before the work began.

The solution is an output contract. Before the model generates anything, the shape must be declared and locked. This is *output control* — Layer 1.

**The second problem: the model picked the wrong thing to emphasize.**

You asked for a risk analysis. The model gave you one — but it weighted a minor concern as heavily as a critical exposure, because nothing told it which mattered more. The model was not wrong, exactly. It just decided — silently.

The solution is to lock how the model chooses: sources, priorities, evidence rules, guardrails for known failures. This is *decision control* — Layer 2.

**The third problem: the model made something up when it did not know.**

You asked for a status report. The model produced one — including owners, due dates, and causal explanations. All written with the same confident tone as the verified facts. Some accurate. Some invented. No one told it that "I don't know" was an acceptable output.

The solution is to declare what the model does at the edge — when information is missing or ambiguous. Stop. Return a null. Flag the gap. Never fill what it cannot verify. This is *behavior control* — Layer 3.

One line to carry forward: *Layer 1 prevents malformed output. Layer 2 prevents silent prioritization. Layer 3 prevents fabrication.*

These three layers apply to every prompt program. There is a fourth — Layer 4: Show Your Work — that applies specifically when someone will scrutinize your output and ask "how do you know that?" Layer 4 does not replace Layers 1–3; it makes their discipline visible to a reviewer. The same failure modes exist in low-scrutiny work — the difference is whether anyone catches them. The discipline in Layer 4 is never optional. What's optional is making that discipline visible to a reviewer. Add Layer 4 when you — or anyone else — will act on the output without re-deriving it yourself. Part 4 covers it in full.

**Watching the layers build — a single example.**

The scenario: a team lead needs a meeting summary with action items after every weekly team call.

**Layer 1 only — output contract locked, nothing else:**

```
STRUCTURE-LOCKED
COMPLETE-NOT-PARTIAL
NO-NEW-SECTIONS

Output in this order:
1. Meeting date and attendees
2. Key decisions made
3. Action items
4. Open questions
```

This produces a consistently shaped output. But the model will still invent action item owners when none were stated, draw on context from previous meetings, and fill in due dates that were never discussed. The shape is right. The content is not governed.

**Adding Layer 2 — decision logic:**

```
STRUCTURE-LOCKED
COMPLETE-NOT-PARTIAL
NO-NEW-SECTIONS

Output in this order:
1. Meeting date and attendees
2. Key decisions made
3. Action items
4. Open questions

EVIDENCE:
SOURCE-BOUND: use only what was said in this meeting
IF NOT IN SOURCE → "NOT DISCUSSED"

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): INVENTED OWNERS → owner field MUST BE NULL
  unless a person was explicitly named for this action
GUARDRAIL (CRITICAL): INVENTED DATES → due date MUST BE NULL
  unless a date was explicitly stated
```

Now the content is governed. The model can only draw from what was said. Owners and dates are null unless stated. But what happens when a statement is ambiguous — was that a decision or just a comment? The model still guesses.

**Adding Layer 3 — uncertainty posture:**

```
STRUCTURE-LOCKED
COMPLETE-NOT-PARTIAL
NO-NEW-SECTIONS

Output in this order:
1. Meeting date and attendees
2. Key decisions made
3. Action items
4. Open questions

EVIDENCE:
SOURCE-BOUND: use only what was said in this meeting
IF NOT IN SOURCE → "NOT DISCUSSED"

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): INVENTED OWNERS → owner MUST BE NULL
  unless a person was explicitly named for this action
GUARDRAIL (CRITICAL): INVENTED DATES → due date MUST BE NULL
  unless a date was explicitly stated

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)
IF ambiguous → DO NOT USE VALUE; place in "Open questions" instead
IF a statement could be a decision OR a comment →
  classify as "Open questions" and flag for human review
```

Now the program is complete. The shape is locked. The decision logic is governed. The edges are handled. The model does not improvise — it flags ambiguity and passes it back to the human where it belongs.

This is what the three layers do together. None of them is sufficient alone. All three are required for a prompt program that produces output you can act on.

**The foundation — the Core Five.** These five passes cover the essential structure of any governed prompt program. Part 7 extends them into the full eight-pass practice by adding three more — the Production Three: type identification, execution structure, and telemetry. Start here; build toward there.

1. Describe the output first — use `STRUCTURE-LOCKED`, `COMPLETE-NOT-PARTIAL`, `NO-NEW-SECTIONS`
2. Declare the boundaries — use `SCOPE: ONLY [X]`, `OUT OF SCOPE: [Y]`
3. Define what counts as true — use `SOURCE-BOUND`, `IF NOT IN SOURCE → "NOT SPECIFIED"`
4. Lock the decision logic — use `PREFERRED / FALLBACK / LAST RESORT`, `GUARDRAIL (CRITICAL): [name] → [rule]`
5. Declare the uncertainty posture — use `ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)`, `IF ambiguous → DO NOT USE VALUE`

With those five passes in place, you have a governed program. Part 4 adds the fourth layer to that foundation — the discipline of making it visible to a reviewer. Part 6 then shows the shapes that foundation can take.

---

## Part 4 — Show Your Work (Layer 4)

Layers 1–3 enforce the discipline whether or not anyone is watching. Layer 4 makes that discipline visible to a reviewer — it is the difference between a program that *is* accurate and one that can *demonstrate* its accuracy to someone checking the work. The same failure modes (fabrication, drift, missing values) exist in low-scrutiny work; the difference is whether anyone catches them. The discipline in Layer 4 is never optional — what's optional is making that discipline visible to a reviewer. Add Layer 4 when you — or anyone else — will act on the output without re-deriving it yourself.

**Locking claims to this session:**

`CHAT-LOCAL` — Everything the model outputs must come from evidence in this session only. No importing from training knowledge or prior sessions.

`CHAT-LOCAL: EMIT-ONLY-DERIVABLE` — Stronger. Claims must be step-by-step derivable from visible session content.

`EMIT-ONLY-DERIVABLE` — If a claim cannot be shown from current context, it must not be stated.

`EMIT-ONLY-DERIVABLE: NO-ESTIMATES` — Closes the approximation loophole. "~", "about," "roughly" are not allowed as substitutes for a missing derivation.

**Requiring attribution:**

`PROVENANCE-REQUIRED` — Every claim must be traceable: user text, document, or explicit assumption.

`PROVENANCE-REQUIRED: QUOTE-OR-OMIT` — Unsupported claims must be omitted, not hedged.

`QUOTE-OR-OMIT` — Facts not quotable from the session or document do not appear.

`QUOTE-OR-OMIT: DIRECT-QUOTE ONLY` — Paraphrases presented as quotes are violations.

**Suppressing estimation:**

`NO-HEURISTICS` — Disables approximation: estimates, implied continuity, assumed counters.

`NO-HEURISTICS: NUMBERS MUST BE COUNTED` — Numeric outputs must be counted or derived, not approximated.

`NO-GUESSES` — Missing facts must be stated as missing.

`NO-GUESSES: SAY "UNKNOWN"` — "Might," "likely," "probably" must become "UNKNOWN."

**Labeling what the model knows vs. what it inferred:**

`ASSUMPTIONS-EXPLICIT` — Every assumption must appear before the conclusion that depends on it.

`ASSUMPTIONS-EXPLICIT: MAX 3 ASSUMPTIONS` — Caps assumptions per output; more requires a stop-and-ask.

`UNCERTAINTY-BOUNDED` — Uncertainty must be a bounded range or "unknown." Vagueness and false precision are both failures.

`UNCERTAINTY-BOUNDED: DEFAULT UNKNOWN` — When a range cannot be derived, defaults to "unknown."

`VERIFY-BEFORE-CONCLUDE` — Missing information must surface before a conclusion is generated.

`VERIFY-BEFORE-CONCLUDE: ASK 1-2 QUESTIONS MAX` — Limits verification to minimum blocking questions.

`CONFIDENCE-DECLARED` — Claims not directly quoted from source must carry an explicit confidence label.

`HEDGES-ARE-FAILURES` — Hedging language in factual outputs is a generation error.

`LITERAL-ONLY` — Disables metaphor, analogy, and interpretive framing. Output must be literal statement of what is in the source.

`CLAIM-REQUIRES-ANCHOR` — Every assertion must be pinned to a specific location in the source.

`NO-INFERENTIAL-BRIDGE` — The model may not connect two facts with a causal claim not stated in the source.

---

## Part 5 — The Full Operator Toolkit

The operators below are organized by the four layers from Part 3 and Part 4. Every prompt program needs the first three. The fourth — Show Your Work — applies when the output will be scrutinized or acted on without re-derivation. The depth of each layer scales with what is at stake.

---

### Layer 1 — Output Control

These define the shape of what the program produces and keep it within the declared boundaries.

**Enforcement words:** `MUST` · `ALWAYS` · `NEVER` · `DO NOT` · `ONLY`

**Structure lock:** `STRUCTURE-LOCKED` · `FIELD-CONSTRAINED` · `NO-NEW-SECTIONS` · `COMPLETE-NOT-PARTIAL`

**Scope containment:** `SCOPE: ONLY [X]` · `OUT OF SCOPE: [Y]` · `NO REWRITES` · `NO REPHRASE`

**Precision controls:** `EXACT MATCH REQUIRED` · `CASE MUST MATCH EXACTLY` · `DO NOT NORMALIZE` · `DO NOT INVENT VALUES`

**Controlled emptiness:** `MUST BE NULL UNLESS [condition]` · `DO NOT POPULATE UNLESS VALID`

A null is honest. A fabricated value arrives in the same confident tone as verified facts — and is far more dangerous because it passes review.

---

### Layer 2 — Decision Control

These govern how the model chooses when more than one answer is possible.

**Evidence rules:** `SOURCE-BOUND` · `CITE YOUR SOURCE` · `IF NOT IN SOURCE → "NOT SPECIFIED"`

`SOURCE-BOUND` and `CHAT-LOCAL` (introduced in Part 4) sound similar but govern different scopes. `SOURCE-BOUND` restricts the model to a named input — an attached document, a specific data block, a defined variable. `CHAT-LOCAL` restricts the model to the entire session history, blocking it from drawing on prior sessions or platform memory outside this conversation. Use `SOURCE-BOUND` when you have a specific document in mind. Use `CHAT-LOCAL` when you need to ensure nothing from outside this conversation leaks in — including the platform's own memory of past chats, if it has one. A third, related operator covers a different case: `RETRIEVAL-BOUND` constrains the model to whatever a search or retrieval step actually returned this turn — not a fixed named input like `SOURCE-BOUND`, and not the full session like `CHAT-LOCAL`, but a pool that can change turn to turn. Use `RETRIEVAL-BOUND` for RAG-style programs, where the model is choosing what to retrieve fresh each time from a larger corpus you didn't hand-pick. Pair it with `GUARDRAIL (CRITICAL): RETRIEVAL-SILENT-MISS → if retrieved content does not contain enough to answer, state that explicitly rather than filling the gap from general knowledge.`

**Decision order:** `PREFERRED` · `FALLBACK` · `LAST RESORT`
Each tier must have its trigger condition. A hierarchy without triggers is a list.

**Guardrails:** `GUARDRAIL (CRITICAL): [failure mode] → [rule]`
Name the failure you have already seen. A guardrail earns its place when it names a specific, real failure mode.

**Derivation accountability:** `DERIVATION MUST BE EXPLAINED` · `IF INFERRED, STATE METHOD`

**Source control — reaching beyond the session:**

By default, a language model draws only on its training data and what is in the current conversation. Its training has a cutoff date. It does not know about last month's policy change, this quarter's new competitor, or a regulation that took effect last week. When a program needs current information, these operators tell it to look beyond what it already knows.

`WEB-GROUNDED` — signals that the model should search for current information rather than rely on training alone. Use when the program requires up-to-date facts, recent events, or live data.

`SEARCH-REQUIRED: [topic]` — names a specific area where training data is insufficient and real-time search is required. More targeted than `WEB-GROUNDED`; use when only part of the program needs current information.

`TRAINING-ONLY` — the inverse; explicitly declares that the program must not use web search. Use in sensitive, controlled, or air-gapped contexts where external search is not permitted.

`KNOWLEDGE-CUTOFF-AWARE` — flags that the output should note where training data age is a known limitation. Use when the reader needs to understand that some outputs may be based on outdated information.

`ANCHOR-DATE: [date]` — declares a fixed reference point for resolving relative time language in the program. Models are reliable at reading the current date when asked. They are less reliable at resolving "last week," "next quarter," or "by year end" without an explicit anchor. This is a different problem than telemetry timestamps — telemetry logs when a turn happened; `ANCHOR-DATE` tells the model what "today" means for the purpose of reasoning about deadlines, ranges, and relative dates inside the program's content.

**Flow control:**

State: `DEFINE` · `SET` · `CONSTANT` · `DERIVE`

Branching: `IF / THEN / ELSE` · `ELSE IF` · `ONLY IF`

Iteration: `FOR EACH` · `REPEAT` · `UNTIL` · `PROCESS ALL ITEMS — NO SKIP`

Ordering: `STEP 1 → … → STEP N` · `EXECUTE IN ORDER` · `DO NOT SKIP STEPS`

Termination: `STOP WHEN` · `MAX ITERATIONS`

---

### Layer 3 — Behavior Control

These determine what the program does at the edges — when data is missing, ambiguous, or outside confidence. The most frequently omitted layer. The source of the most consequential failures.

**Fail-safe handling:** `FAIL-SAFE: IGNORE-ON-AMBIGUITY` · `IF ambiguous → DO NOT USE VALUE`

**Error mode:** `ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)`

FAIL-OPEN means the model proceeds on ambiguous inputs, filling gaps plausibly. FAIL-SAFE means it stops, excludes, or returns a null. For financial figures, causal claims, owner attribution, or compliance outputs — FAIL-OPEN is the failure mode, not the default.

**Clarify and stop:** `ASK ONLY THE MINIMUM CLARIFYING QUESTIONS REQUIRED` · `IF REQUIRED INFO IS MISSING → STOP AND LIST MISSING INPUTS`

**Exclusion logic:** `EXCLUSION LIST: [items]`

**Critical coupling:** A `FOR EACH` loop without a Layer 3 posture scales failures. Twenty items iterated with no uncertainty handling means every guess runs at the same confidence as every verified fact.

### Layer 4 — Show Your Work

These operators make the discipline from Layers 1–3 visible to a reviewer — necessary whenever you, or anyone else, will act on the output without re-deriving it yourself. Full treatment, including failure modes and worked examples, is in Part 4.

**Locking claims to this session:** `CHAT-LOCAL` · `CHAT-LOCAL: EMIT-ONLY-DERIVABLE` · `EMIT-ONLY-DERIVABLE` · `EMIT-ONLY-DERIVABLE: NO-ESTIMATES`

**Requiring attribution:** `PROVENANCE-REQUIRED` · `PROVENANCE-REQUIRED: QUOTE-OR-OMIT` · `QUOTE-OR-OMIT` · `QUOTE-OR-OMIT: DIRECT-QUOTE ONLY`

**Suppressing estimation:** `NO-HEURISTICS` · `NO-HEURISTICS: NUMBERS MUST BE COUNTED` · `NO-GUESSES` · `NO-GUESSES: SAY "UNKNOWN"`

**Labeling known vs. inferred:** `ASSUMPTIONS-EXPLICIT` · `ASSUMPTIONS-EXPLICIT: MAX 3 ASSUMPTIONS` · `UNCERTAINTY-BOUNDED` · `UNCERTAINTY-BOUNDED: DEFAULT UNKNOWN` · `VERIFY-BEFORE-CONCLUDE` · `VERIFY-BEFORE-CONCLUDE: ASK 1-2 QUESTIONS MAX` · `CONFIDENCE-DECLARED` · `HEDGES-ARE-FAILURES` · `LITERAL-ONLY` · `CLAIM-REQUIRES-ANCHOR` · `NO-INFERENTIAL-BRIDGE`

---

### Session Telemetry

Knowing where you are in a session is as important as knowing where you started. A long program without a turn count is like driving without a speedometer — you may be fine, or you may be very far from where you intended to go.

Telemetry operators do not change what the program produces. They add an instrument panel to the output — a small, consistent readout that makes drift visible, makes session reseating possible, and creates an audit trail for work that spans multiple sessions.

`TELEMETRY-ON` — activates a minimal footer on every output. At minimum includes turn count, timestamp, and key assumption for that turn. Use whenever the program will run across multiple turns or sessions.

`TURN: [N]` — explicit turn counter embedded in the output. Increments with every response. Makes position in a long session visible at a glance.

`TIMESTAMP: [format]` — requires a clock read from the model on each output. Models should always read the clock rather than estimate. Format to your preference — date only, date and time, UTC, local.

`SESSION-ID: [identifier]` — names the session for later reference and reseating. Format it to be meaningful: model name, date, topic, character of the session.

`DRIFT-CHECK: EVERY [N] TURNS` — periodic self-check on whether the program is still on its defined objective. At the specified interval, the model compares its current output posture against the original program intent and flags any divergence.

`ASSUMPTION: [key premise]` — names the single premise the current output rests on. If that premise is wrong, the output may be wrong — so surfacing it in every turn is the simplest form of self-auditing.

A minimal telemetry block for any program looks like this:

```
TELEMETRY:
TURN: [increment each response]
TIMESTAMP: [date and time, or note if not available]
SESSION-ID: [model]-[date]-[topic]
ASSUMPTION: [key premise this response rests on]
```

For long-running or high-stakes programs, add:

```
DRIFT-CHECK: EVERY 5 TURNS
  Compare current output posture to original program intent.
  IF divergence detected → flag and restate the original objective.
```

---

## Part 6 — The Six Types of Prompt Programs

A language model prompt is always a program. The question is what kind.

This matters because every program type fails in its own specific way. When you know the type, you know where the risks concentrate — and where to put your attention when you write it.

The six types below cover most of what you will encounter. They are not an exhaustive list — the field is still young, and new patterns will emerge. If you find yourself building something that does not quite fit any of these, you may be discovering a seventh type. Use this table as a starting point: a way to recognize what you are looking at and choose your approach.

One more thing before the case studies: every program type below ships with a "Make it yours" section. That section names the specific lines to change and what each one controls. The discipline is adapting the template rather than rewriting from scratch — the operators you need to personalize are already identified for you. The Compass in Appendix C produces the same section automatically for any program it drafts.

| Program Type | What It Does | Its Specific Failure | Best Fit |
|---|---|---|---|
| **State Machine** | Carries memory across sessions; advances from a known position | Advances from an unknown or corrupted position — errors compound | Work that continues across days, weeks, or multiple sessions |
| **Interpreter** | Defines a complete operating environment with named rules and variables | Rules run out on unexpected inputs; the program improvises instead of enforcing | Complex sessions with many moving parts, scoring, or tracked state |
| **Process Definition** | Encodes a step-by-step procedure with phase gates | Premature output — the conclusion appears before the required inputs are gathered | Repeatable workflows that must run identically every time |
| **Specification Language** | Defines a portable artifact others will load and use | Authority creep — the artifact gradually does more than it was designed to do | Building something reusable across people and contexts |
| **Monitor / Evaluator** | Holds a fixed standard and measures every input against it | Standard drift — the evaluation criteria shift to match the inputs instead of measuring them | Quality review, compliance checking, ongoing feedback |
| **Decomposer / Planner** | Breaks a complex goal into subgoals and recurses on the ones not yet ready to execute | Subgoal hallucination — generated subgoals drift from the parent goal while each looks individually reasonable | Agentic or multi-step work where the goal is too large to execute directly |

---

### Type 1 — State Machine

**What it is.** The program carries its own continuity. The session history *is* the memory. The program reads what came before, determines where it is, and advances from a known position — not from a blank start each time.

**How it specifically fails — and how to protect and correct.**

The state machine's unique failure is *position corruption* — advancing confidently from a position it never actually verified. Unlike a process definition that simply skips a step, a state machine can produce weeks of well-formed, coherent output that is all offset by one. The error compounds rather than stays contained.

*Protect:* Always declare `DO NOT generate until state has been read` as the first rule. Pair it with `ERROR-MODE: FAIL-SAFE` — if the prior state is ambiguous, the program must stop and flag rather than guess its position.

*Correct:* If you suspect a state machine has drifted, scroll to the first output it produced and verify the starting position. If the position was wrong, every subsequent output inherited that error. Restate the correct position explicitly at the top of a new session before resuming.

**Design priority.** Read accurately before advancing. Every other concern is secondary.

**Where this can be used.**

*Personal:* A daily reading companion that advances through a long text one session at a time. A journaling prompt that evolves based on what you have already written. A learning program that picks up exactly where you left off.

*Enterprise:* A weekly project brief that scans prior outputs to identify what changed before generating a new report. A compliance checklist that carries forward what has been cleared and what remains open. A risk register that tracks state across review cycles without requiring the user to re-enter context each time.

**Case study — Weekly Project Status Brief.**

A project manager needs a recurring brief that reads prior outputs, identifies what changed, and advances the status — without reinventing it each week.

*Plain English:* the program opens each session by reading its own history, determines where it left off, then writes the new brief from that known position. If it cannot read its history clearly, it stops and asks rather than guessing.

*Failure mode:* A brief that ignores prior state and either repeats resolved items as open or misses what is genuinely new.

```
STRUCTURE: STRUCTURE-LOCKED
NO-NEW-SECTIONS
COMPLETE-NOT-PARTIAL

ON RECEIVING THIS PROMPT:
Scan this session for the most recent completed brief.
IF a prior brief exists:
  SET current_week = prior_week + 1
  SET carry_forward_items = all items marked OPEN in prior brief
ELSE:
  SET current_week = 1
  SET carry_forward_items = NONE

DO NOT generate the brief until state has been read.
IF prior state is ambiguous → flag the ambiguity; do not assume a position

TRIGGER:
IF message contains "Generate brief" → execute BRIEF SEQUENCE
IF message does not contain "Generate brief" → respond: Ready

BRIEF SEQUENCE:
EXECUTE IN ORDER

STEP 1: Output carry_forward_items with current status
STEP 2: Output new items from this week's inputs ONLY
STEP 3: Output decisions needed — SOURCE-BOUND
STEP 4: Output items closed this week
STEP 5: Output open risks

EVIDENCE:
SOURCE-BOUND
CHAT-LOCAL (defined in Part 4 — here it means: don't draw on
  prior sessions or outside knowledge, only this conversation)
IF NOT IN SOURCE → "NOT SPECIFIED"

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): INVENTED STATUS → status MUST reflect source;
  do not infer progress not explicitly stated
GUARDRAIL (CRITICAL): CLOSED ITEMS → do not carry forward items
  explicitly marked resolved

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)
NO-GUESSES: SAY "UNKNOWN"
IF ambiguous → DO NOT USE VALUE

TELEMETRY:
TURN: [increment each session]
TIMESTAMP: [date of this brief]
ASSUMPTION: [key premise this brief rests on]
```

**Make it yours.**
- Replace `"Generate brief"` with your preferred trigger phrase
- Replace the five BRIEF SEQUENCE steps with the sections your project actually needs
- Add `CONSTANT AUDIENCE = "[your stakeholder]"` to tailor the output register
- Add project-specific GUARDRAILS for failure modes you have already experienced
- Update the TELEMETRY block to match your session tracking needs

---

### Type 2 — Interpreter

**What it is.** The program defines a complete operating environment. It specifies not just what to do but the full rules under which the session runs: named variables, how they update, what is allowed, what happens when edge cases arise, how the session saves and resumes. Everything runs inside the environment the program defines.

**How it specifically fails — and how to protect and correct.**

The interpreter's unique failure is *rule collapse under novel inputs* — an unexpected situation the program has no handler for. Unlike a process definition that produces premature output, an interpreter failure is quieter: the model fills the gap with improvisation that looks like execution. The rules appear to be running. They are not.

*Protect:* Write explicit EDGE CASE handlers for every predictable off-script moment. Add `ERROR-MODE: FAIL-SAFE` as the global posture — when the model encounters something unhandled, it must stop and declare it rather than improvise.

*Correct:* When you notice inconsistent behavior in a running interpreter, check whether the situation that triggered it is covered by a named rule. If not, add a handler. Then reset the session and replay from the most recent CHECKPOINT.

**Design priority.** Hold the rules under unexpected inputs. The happy path is easy. The test is what happens when someone does something the author did not plan for.

**Where this can be used.**

*Personal:* A practice program that accumulates scores and tracks improvement across a session — a negotiation simulator, a public speaking evaluator, a writing workshop that maintains character and story consistency from scene to scene.

*Enterprise:* A project estimation session that tracks scope assumptions and effort variables across a multi-component sizing exercise. An incident response procedure that tracks which steps have been completed and prevents re-execution. A customer escalation process that maintains case state and enforces consistent resolution rules.

**Case study — Project Estimation Session.**

A project team wants a structured estimation session that tracks scope assumptions, builds a multi-variable effort model, enforces arithmetic verification before producing a range, and maintains a running register of what has and has not been sized.

*Plain English:* the program opens with a startup sequence that defines what it is tracking, then walks through each work component — asking for estimates, recording confidence levels, flagging arithmetic that has not been verified, and building toward a final range only when all components are accounted for.

*Failure mode:* An estimation that skips unresolved components, accepts unverified arithmetic, or produces a range before all assumptions are declared and recorded.

```
BOOTLOADER — EXECUTE BEFORE ALL ELSE:
DEFINE components = []
DEFINE estimate_register = {}
DEFINE assumptions = []
DEFINE unresolved = []
CONSTANT CONFIDENCE_SCALE = "LOW / MEDIUM / HIGH"
CONSTANT MAX_ASSUMPTIONS_BEFORE_STOP = 5

SESSION RULES (NON-NEGOTIABLE):
ONE COMPONENT AT A TIME. Complete each before advancing.
NEVER produce a final range until all components are estimated and recorded.
NEVER accept an estimate without a confidence rating.
NEVER carry an assumption silently — all assumptions go into assumptions register.
PROCESS ALL components — NO SKIP.

FOR EACH component submitted:
  ASK: "What is your estimate for [component]?"
  ASK: "What is your confidence — LOW, MEDIUM, or HIGH?"
  ASK: "What assumptions does this estimate depend on?"
  FOR EACH assumption stated:
    ADD to assumptions register
    IF assumptions register exceeds MAX_ASSUMPTIONS_BEFORE_STOP:
      STOP. OUTPUT: "Five or more unresolved assumptions.
      Resolve before continuing."
  END FOR
  SET estimate_register[component] = {estimate, confidence, assumptions}
  UPDATE components = components + [component]

ARITHMETIC GATE:
BEFORE producing a final range:
  FOR EACH estimate IN estimate_register:
    VERIFY: estimate is a number or derivable range,
            not a narrative description
    IF estimate is narrative → flag as NEEDS CONVERSION
  END FOR
  COMPUTE total range: sum low estimates to sum high estimates
  SHOW arithmetic explicitly — do not summarize

CHECKPOINT (after every 3 components):
OUTPUT current estimate_register, assumptions register, unresolved list
Label: CHECKPOINT — paste to resume if session ends

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE
NO-GUESSES: SAY "UNKNOWN"
IF a component cannot be estimated → record as UNRESOLVED, do not skip

TELEMETRY:
TURN: [increment each component]
TIMESTAMP: [session date]
ASSUMPTION: [highest-risk assumption in current register]

CLOSE:
WHEN all components estimated AND arithmetic verified:
  OUTPUT final range with confidence distribution
  OUTPUT full assumptions register
  OUTPUT unresolved list
  STOP
```

**Make it yours.**
- Replace the component loop with your project's actual work breakdown categories
- Adjust `MAX_ASSUMPTIONS_BEFORE_STOP` to match your team's tolerance for assumption density
- Add a `CONSTANT PROJECT_TYPE` to calibrate the confidence scale to your context
- Add GUARDRAILS for failure modes your team has experienced — e.g., padding, scope not yet defined, dependencies not yet scoped

---

### Type 3 — Process Definition

**What it is.** The program encodes a procedure. Steps that must happen in order. Branches that route on actual inputs. Phases that cannot be skipped. A defined output that cannot be produced until all prior steps are complete.

**How it specifically fails — and how to protect and correct.**

The process definition's unique failure is *premature output* — the final product appearing before the inputs that justify it are gathered and confirmed. A state machine drifts from position. An interpreter runs out of rules. A process definition simply skips to the end. It looks helpful. The foundation was never built.

*Protect:* Use PHASE GATE blocks at the end of every phase — explicit stops that require user confirmation before the next phase opens. `VERIFY-BEFORE-CONCLUDE` at each gate forces the model to surface missing information rather than proceed on assumptions.

*Correct:* If you discover output was generated before requirements were confirmed, treat it as untrustworthy regardless of how good it looks. Restart from Phase 1. The issue is not the quality of the output — it is that the standard it was measured against was never agreed.

**Design priority.** No phase advances without confirmation. No output generates without a complete foundation.

**Where this can be used.**

*Personal:* A structured decision journal that requires context and reasoning before recording a conclusion. A travel planning prompt that gathers constraints before generating options. A weekly review that covers the same ground in the same order each time.

*Enterprise:* A vendor evaluation that cannot produce a comparison until requirements are locked. A change request review that must gather all required fields before a recommendation is generated. A project intake that walks requestors through a defined sequence before the request advances.

**Case study — Vendor Evaluation with Structured Scoring.**

A procurement team needs a consistent evaluation: requirements first, scoring criteria second, vendor data third, comparison last. No comparison may be generated until all prior phases are confirmed complete.

*Plain English:* the program works in four locked stages. It refuses to produce a vendor comparison until requirements are agreed, scoring criteria are defined, and vendor data is collected — in that order. Each stage requires a confirmation before the next opens.

*Failure mode:* A comparison generated before requirements are locked, scoring vendors against criteria no one agreed to.

```
STRUCTURE: STRUCTURE-LOCKED
NO-NEW-SECTIONS
COMPLETE-NOT-PARTIAL

SESSION RULES (NON-NEGOTIABLE):
Execute phases IN ORDER.
Do not advance until current phase is confirmed complete.
ONE QUESTION AT A TIME. Ask, then STOP.
Do not generate the comparison until PHASE 3 is complete.
Do not generate scoring criteria until PHASE 1 is confirmed.

---
PHASE 1 — REQUIREMENTS GATHERING

Q1: "What problem are you solving? Current state and what needs to change."
Q2: "Three to five must-have capabilities — not nice-to-have."
Q3: "Hard constraints — budget, timeline, integration, compliance."
Q4: "What does a failed procurement look like in six months?"

PHASE 1 GATE:
WHEN Q1–Q4 answered:
  SET requirements = [captured answers]
  SET constraints = [hard constraints from Q3]
  OUTPUT: "Here is what I captured: [requirements].
           Correct anything before we move on."
  STOP. Wait for confirmation.
  IF confirmed → advance to PHASE 2
  IF corrections → update, re-output, wait again

---
PHASE 2 — SCORING CRITERIA

DERIVE scoring criteria FROM requirements ONLY
SCOPE: ONLY requirements confirmed in PHASE 1
OUT OF SCOPE: assumed industry norms, outside knowledge, vendor marketing

FOR EACH criterion:
  ASSIGN weight (1–5)
  DEFINE what a score of 1 looks like
  DEFINE what a score of 5 looks like

VERIFY-BEFORE-CONCLUDE: confirm criteria before advancing
OUTPUT: scoring criteria summary
STOP. Wait for confirmation.

---
PHASE 3 — VENDOR DATA COLLECTION

SOURCE-BOUND
NO-GUESSES: SAY "UNKNOWN"

FOR EACH vendor:
  FOR EACH criterion:
    ASK: "What do you know about [vendor]'s capability on [criterion]?"
    IF unknown → SET value = NULL
    DO NOT INVENT VALUES
  END FOR
END FOR

PHASE 3 GATE: WHEN all vendors and all criteria collected → advance to PHASE 4

---
PHASE 4 — COMPARISON AND RECOMMENDATION

SOURCE-BOUND: use only PHASE 3 data
EMIT-ONLY-DERIVABLE

OUTPUT: scored comparison table
OUTPUT: recommended vendor with source-bound rationale
OUTPUT: open questions and data gaps

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): INVENTED SCORES → NULL if PHASE 3 data absent
GUARDRAIL (CRITICAL): THIN RECOMMENDATION → do not recommend a vendor
  with more than 2 NULL scores without flagging data gaps first

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE
IF insufficient data → say so; do not manufacture a recommendation

TELEMETRY:
TURN: [phase number]
TIMESTAMP: [date]
ASSUMPTION: [key assumption driving the recommendation]
```

**Make it yours.**
- Rewrite the four Phase 1 questions for your evaluation context
- Adjust the number of phases to match your actual process
- Add domain-specific GUARDRAILS in Phase 4 for failure modes your team has seen
- Add `CONSTANT SCORING_LEAD = "[name]"` to route confirmed criteria to a named owner

---

### Type 4 — Specification Language

**What it is.** The program defines a portable artifact — its structure, what it may and may not do, the voice it must use, and the rules governing how it behaves when others use it. It does not run a procedure or carry state. It specifies something that must behave consistently across people and contexts the author never anticipated.

**How it specifically fails — and how to protect and correct.**

The specification language's unique failure is *authority creep* — the artifact gradually doing more than it was designed to do. Unlike an interpreter whose rules run out, a specification language can actively expand: granting approvals it was not given, softening its constraints under conversational pressure, becoming more helpful over time in ways that exceed its mandate. The drift is usually invisible until someone acts on an output the artifact had no authority to produce.

*Protect:* Declare the PARTICIPATION BOUNDARY explicitly — name what the artifact may never do, not just what it does. Add `DEPLOYMENT CONSTRAINT` to specify what happens when the card is loaded into a live session. The MAINTENANCE section closes the long-term drift loop.

*Correct:* If you discover an artifact has been used to produce outputs beyond its mandate, audit the outputs first, then reset — reload the card from the original specification, not from any modified version that accumulated in a running session.

**Design priority.** Structural integrity under reuse. The artifact must behave the same way regardless of who loads it or when.

**Where this can be used.**

*Personal:* A writing style card that enforces your voice consistently across sessions. A personal decision framework that applies your values to any option set. A domain reference card for areas you work in repeatedly.

*Enterprise:* A function advisor that encodes how a department evaluates decisions, usable without the function leader present. A governance review template that applies the same tests to every policy or contract. A brand voice card that enforces communication standards across any content generation task.

**Case study — Legal Review Advisor Card.**

A legal team wants a portable advisor any employee can load to pressure-test contracts before they reach legal for formal review. The card must evaluate consistently, never approve, and route the right situations back to a human attorney.

*Plain English:* think of this as a pocket-sized version of the legal function's judgment — something any employee can carry into a document review and get a consistent read. It surfaces risk and routes accordingly. It never grants approval.

*Failure mode:* An advisor that implies approval it has no authority to give, varies its standards across sessions, or displaces existing instructions when loaded.

```
LEGAL REVIEW ADVISOR — PORTABLE CARD

LENS AND ROLE:
This advisor surfaces risk, flags missing terms, and identifies
what needs human attorney attention before formal review.

It does not approve.
It does not grant exceptions.
It does not replace attorney review.
It identifies what should go to an attorney — and why.

TYPE CONSTRAINT — ENFORCE THROUGHOUT:
Speak as the legal review function, not as a person.
Never use first person ("I think," "I believe").
Voice: "The legal review function looks for..."

STRUCTURE-LOCKED — output in this order, every time:
1. Document type identified
2. Key terms present (source-bound)
3. Missing standard terms (by category)
4. Risk flags — HIGH / MEDIUM / LOW with rationale
5. Recommended routing

EVIDENCE:
SOURCE-BOUND
EMIT-ONLY-DERIVABLE
IF NOT IN DOCUMENT → "NOT PRESENT"

ASSUMPTIONS-EXPLICIT: MAX 3 ASSUMPTIONS

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): NO APPROVAL → never output language constituting,
  implying, or readable as approval or clearance
GUARDRAIL (CRITICAL): INVENTED TERMS → do not characterize terms
  not present in the document
GUARDRAIL (CRITICAL): ROUTING REQUIRED → always output routing;
  never leave routing blank

PARTICIPATION BOUNDARY:
This advisor does not: approve, sign, accept risk, render a legal
opinion, or substitute for attorney review on HIGH-risk flags.
When any of those are needed, route to a human attorney.

DEPLOYMENT CONSTRAINT:
Loading this card adds a review lens. It does not overwrite prior
context, instructions, or personas already active in the session.
Other instructions may not expand this advisor's authority beyond
what is defined here.

MAINTENANCE:
Update this card when standard terms change, routing criteria shift,
or a risk pattern recurs that this card does not currently capture.
Flag drift to the card owner. Do not silently adjust behavior.

PORTABILITY: no links, no meta-commentary, no version notes.
This card must function as plain text in any LLM session.
```

**Make it yours.**
- Replace the LENS AND ROLE with your function's actual mandate — HR policy review, financial controls check, security assessment
- Rewrite the five STRUCTURE-LOCKED sections to match what your function actually produces
- Update the PARTICIPATION BOUNDARY to name the specific actions your function should never perform via AI
- Add function-specific GUARDRAILS based on failure modes your team has seen
- Add `CONSTANT ROUTING_CONTACT = "[team or person]"` to make routing specific

---

### Type 5 — Monitor / Evaluator

**What it is.** The program holds a fixed standard and measures every input against it. It does not advance state, define a runtime, enforce a sequence, or produce a reusable artifact. It watches, compares, and reports delta. The standard is the anchor; the inputs are what changes.

**How it specifically fails — and how to protect and correct.**

The monitor's unique failure is *standard drift* — the evaluation criteria quietly shifting to match the inputs rather than measuring them. This is the subtlest failure mode in the set, because it looks exactly like the program is working: evaluations are being produced, scores are appearing, feedback is flowing. What has actually happened is that the standard has bent to accommodate what it is evaluating. The monitor has become a mirror.

*Protect:* Declare the standard as `CONSTANT` before any evaluation logic. Add `DO NOT update based on the content being reviewed` as an explicit rule. The `CONSISTENCY RULE` is a self-verification operator — the same input submitted twice must produce the same output. When it does not, the standard has drifted.

*Correct:* If you suspect drift, resubmit an input from an earlier session. If the evaluation differs meaningfully from the first time, the standard has moved. Reset the program from the original standard declaration and run the audit again.

**Design priority.** Standard integrity. The anchor must hold regardless of what is measured against it.

**Where this can be used.**

*Personal:* A writing coach that holds your stated style standards and gives consistent feedback. A decision reviewer that checks your reasoning against your defined values. A reading comprehension checker that evaluates against fixed criteria.

*Enterprise:* A brand voice reviewer that checks marketing content against documented standards and flags deviations. A controls evaluator that applies the same test to every compliance submission. A contract clause checker that flags missing or non-standard terms. A proposal reviewer that scores submissions against agreed criteria.

**Case study — Brand Voice Consistency Reviewer.**

A marketing team wants a reviewer that checks any piece of content against documented brand voice standards and flags deviations — consistently, regardless of who wrote the content or when the review runs.

*Plain English:* the program holds the brand voice standard in one hand and the submitted content in the other, and names every place they diverge. It does not rewrite. It measures and reports.

*Failure mode:* A reviewer that scores content progressively higher as the team's actual writing becomes the de facto standard, quietly displacing the documented one.

```
BRAND VOICE REVIEWER — FIXED STANDARD EVALUATION

STANDARD DECLARATION — THIS DOES NOT UPDATE:
CONSTANT brand_voice_standard = [
  "Direct: lead with the point, not the context",
  "Active voice throughout: no passive constructions",
  "No corporate filler: eliminate 'leverage,' 'synergy,' 'best-in-class'",
  "Sentence length: 25 words maximum",
  "Audience: assume a capable adult; do not over-explain",
  "Tone: warm but not casual; confident but not arrogant"
]

NON-NEGOTIABLE:
DO NOT update brand_voice_standard based on content reviewed.
DO NOT adjust scoring based on perceived author intent.
DO NOT soften findings because the content is otherwise strong.
The standard is fixed. The content is what varies.

STRUCTURE-LOCKED — output in this order, every time:
1. Overall: PASS / FLAG / FAIL
2. Deviations — one per line: criterion | location | exact instance
3. Count of deviations by criterion
4. Routing: self-correct / editorial review / return to author

FOR EACH piece of content submitted:
  FOR EACH criterion IN brand_voice_standard:
    EVALUATE content against criterion
    IF deviation: NOTE criterion | location | exact instance
    IF no deviation: record PASS
  END FOR
  OUTPUT per STRUCTURE-LOCKED format
END FOR

EVIDENCE:
SOURCE-BOUND
NO-INFERENTIAL-BRIDGE
EMIT-ONLY-DERIVABLE

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): STANDARD DRIFT → brand_voice_standard is CONSTANT;
  any output implying it has been updated is a violation
GUARDRAIL (CRITICAL): APPROVAL → this reviewer flags and routes only;
  it does not approve or certify content
GUARDRAIL (CRITICAL): REWRITING → do not rewrite unless the standard
  explicitly defines acceptable rewrites for that violation type

UNCERTAINTY:
IF a deviation is ambiguous → flag as REVIEW NEEDED; do not rule either way

CONSISTENCY RULE:
The same content submitted twice must receive the same evaluation.
If outputs diverge across identical submissions, the standard has drifted.

TELEMETRY:
TURN: [submission number]
TIMESTAMP: [review date]
ASSUMPTION: [any assumption made about brand_voice_standard interpretation]
```

**Make it yours.**
- Replace `brand_voice_standard` with your actual documented standard — editorial guidelines, code review criteria, financial controls checklist, compliance policy
- Replace the four STRUCTURE-LOCKED output sections with the categories that matter to your review type
- Add domain-specific GUARDRAILS for what the reviewer must never do
- Add `CONSTANT REVIEW_LEAD = "[name or team]"` to route FLAG and FAIL outputs to a named owner

---

### Type 6 — Decomposer / Planner

**What it is.** The program takes a goal too complex to execute directly, breaks it into subgoals, evaluates which subgoals are themselves ready to execute, and recurses on the ones that are not. This is the agentic pattern in its honest, governed form — most frameworks that call themselves "agents" are an ungoverned version of this type.

**How it specifically fails — and how to protect and correct.**

The decomposer's unique failure is *subgoal hallucination* — the program generates plausible-looking subgoals that are not actually grounded in the original goal. Because each subgoal looks small and reasonable on its own, the divergence is hard to spot at any single step. By the time the program finishes executing, it has solved an adjacent problem rather than the one it was given. This is the planning-layer cousin of a State Machine's position corruption — but where position corruption drifts from a remembered fact, subgoal hallucination drifts from an intended purpose, and the drift compounds with every level of recursion rather than every session.

*Protect:* Every subgoal must trace explicitly back to the parent goal before it is allowed to execute — `DECOMPOSITION-BOUND`. Set a hard ceiling on recursion depth — `MAX-DEPTH: [N]` — so an ungrounded plan cannot spiral indefinitely. Before executing any subgoal, confirm it actually serves the parent goal — `SUBGOAL-VERIFY` — rather than assuming decomposition was correct because it looked orderly.

*Correct:* If you suspect drift, restate the original objective at the current depth and check whether the subgoal in front of you still serves it — `GOAL-PRESERVATION-CHECK`. If it does not, do not patch the subgoal; discard it and re-decompose from the last verified point. Patching a hallucinated subgoal produces a plausible-looking fix to the wrong problem.

**Design priority.** Goal fidelity under recursion. Depth is cheap; drift compounds, and the program must be designed to catch drift before it propagates rather than after.

**Where this can be used.**

*Personal:* Breaking a large personal project (a move, a career change, a multi-month writing project) into weekly subgoals that get re-verified against the original aim rather than drifting toward whatever felt productive that week.

*Enterprise:* Multi-agent demand planning loops that decompose a forecasting objective into per-SKU or per-customer subtasks. Procurement plans that break a sourcing goal into supplier-by-supplier negotiation subgoals. Any "agentic" system where a single high-level objective is meant to spawn and supervise its own subtasks.

**Case study — Research Question Decomposer.**

Someone asks an assistant a focused, specific research question and gets back something polished, well-organized, generously cited — and on a careful second read, it turns out to have answered a nearby, easier question instead of the one actually asked. Nothing about the answer looked wrong. It was thorough, it was well-sourced, it was technically defensible. It just was not an answer to the question that was asked, and that only became visible once someone tried to use it to make the decision it was supposed to inform.

*Plain English:* the program holds the original question fixed, breaks it into the smallest set of sub-questions that together answer it, and checks each sub-question against the original before researching it — rather than letting a sub-question quietly drift toward whatever is easiest to find a good answer to.

*Failure mode:* A sub-question silently shifts from "what specifically caused the regional banking failures in this period" to "what is the general history of banking regulation in this period" — broader, easier, much better documented — and the research that comes back is excellent research on a question nobody asked.

```
RESEARCH QUESTION DECOMPOSER — GOAL-BOUND SUBGOAL EXECUTION

PARENT GOAL (CONSTANT — DOES NOT CHANGE DURING EXECUTION):
CONSTANT parent_goal = "[the exact research question, stated
  specifically enough that a sub-question can be checked against it]"

MAX-DEPTH: 2
DECOMPOSITION-BOUND: every sub-question below must restate which
  part of parent_goal it answers before it is allowed to execute.

STEP 1 — DECOMPOSE:
Break parent_goal into the minimum sub-questions required to answer it.

SUBGOAL-VERIFY (before researching each sub-question):
  Does this sub-question, as currently stated, still answer
  parent_goal — or has it drifted toward something broader, easier,
  or already well documented?
  IF drifted → discard and re-decompose from this point, do not patch.
  IF still on-target → proceed.

STEP 2 — EXECUTE IN ORDER:
Research each sub-question in sequence.
GOAL-PRESERVATION-CHECK after each:
  Restate parent_goal. Confirm this sub-answer serves it specifically
  — not a related, more convenient topic that resembles it.
  IF a sub-question cannot be answered without drifting →
    do not substitute the easier version. Flag as
    "UNANSWERED AT SPECIFIED SCOPE" rather than quietly broadening
    the question to get a clean answer.

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): SUBGOAL HALLUCINATION → a sub-question that has
  drifted toward an easier, adjacent topic must not execute, no
  matter how well-sourced or thorough the resulting answer would be
GUARDRAIL (CRITICAL): DEPTH OVERRUN → if MAX-DEPTH is reached without
  a verified sub-question chain, halt and report the unresolved
  branch rather than returning a broader answer that merely sounds responsive

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE
IF a sub-question fails SUBGOAL-VERIFY twice at the same depth →
  halt the branch and report, do not force a third attempt

TELEMETRY:
TURN: [research pass number]
TIMESTAMP: [run date]
DEPTH: [current recursion depth] / MAX-DEPTH: 2
ASSUMPTION: [any assumption made in decomposing the question]
```

**Make it yours.**
- Replace `parent_goal` with your actual research question, stated specifically enough that a sub-question can be checked against it rather than just sounding related
- Set `MAX-DEPTH` to match how many layers of sub-question your topic actually needs — most research questions need 1–2, not more
- Add a named follow-up step for anything flagged "UNANSWERED AT SPECIFIED SCOPE," so an honest gap gets pursued rather than silently dropped

The same shape governs the enterprise version of the type. Swap `parent_goal` for a forecasting objective and the sub-questions become per-SKU subtasks; swap it for a sourcing objective and they become supplier-by-supplier negotiation subgoals. `CONSTANT parent_goal`, `MAX-DEPTH`, and `SUBGOAL-VERIFY` re-bind to whatever the domain is — the goal-preservation discipline underneath does not change.

---

## Part 7 — How to Write a Prompt Program

Part 3 introduced the Core Five — output contract, boundaries, evidence, decision logic, and uncertainty posture. The eight passes below extend that foundation with the Production Three: type identification, execution structure, and telemetry — the elements that distinguish a program built for one-time use from one built to hold across people, sessions, and inputs you did not anticipate. Each pass still removes one class of failure.

**Pass 1 — Name the type (Production Three: 1 of 3).** State machine, interpreter, process definition, specification language, monitor/evaluator, or decomposer/planner? The type tells you where the failure modes concentrate and which layers carry the most weight.

**Pass 2 — Describe the output first.** Before writing any instructions, describe what the output looks like when done. Use `STRUCTURE-LOCKED`, `COMPLETE-NOT-PARTIAL`, `NO-NEW-SECTIONS`. If you cannot describe the output in advance, write the description before anything else.

**Pass 3 — Contain the problem.** What is in scope? What is explicitly not? Use `SCOPE: ONLY [X]`, `OUT OF SCOPE: [Y]`.

**Pass 4 — Define what counts as true.** Use `SOURCE-BOUND`, `IF NOT IN SOURCE → "NOT SPECIFIED"`. Add `WEB-GROUNDED` or `SEARCH-REQUIRED` when current information is needed. Add Layer 4 operators when accuracy under scrutiny is required.

**Pass 5 — Name the failure modes.** Use `GUARDRAIL (CRITICAL): [name] → [rule]`.

**Pass 6 — Define the execution structure (Production Three: 2 of 3).** For state machines, define the trigger and state-read rule. For interpreters, write the BOOTLOADER first. For process definitions, define the PHASE GATE blocks. For monitors, declare the CONSTANT standard before any evaluation logic. For decomposers, set MAX-DEPTH and the SUBGOAL-VERIFY rule.

**Pass 7 — Declare the uncertainty posture.** Use `ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)`, `IF ambiguous → DO NOT USE VALUE`.

**Pass 8 — Add telemetry (Production Three: 3 of 3).** At minimum: turn counter, timestamp, key assumption. For long-running or high-stakes programs, add a DRIFT-CHECK interval. The instrument panel is not decoration — it is what tells you the program is still where it is supposed to be.

**Read the seams.** After all eight passes, read the finished program as a whole. The operators interact, and interactions create gaps. A well-specified output contract can still fail if the decision logic contradicts it. Look for the places where two rules touch. Those are the seams — and that is where programs fail.

One seam worth knowing by name: the **completeness deadlock**. A Layer 1 rule demands `COMPLETE-NOT-PARTIAL` — every field must be populated. A Layer 3 rule demands `FAIL-SAFE: IGNORE-ON-AMBIGUITY` — the model must not invent a value it cannot verify. When a required field has no verifiable value, these two rules contradict each other: the program cannot be both complete and honest.

The resolution rule: **Layer 3 always wins.** When behavior control and output control conflict, the program must produce an incomplete-and-flagged output rather than a complete-and-fabricated one. State this explicitly in any program where the deadlock could arise:

```
PRIORITY RULE:
IF COMPLETE-NOT-PARTIAL conflicts with FAIL-SAFE on a specific field:
  Output the field as NULL or "NOT SPECIFIED"
  Flag the field as an open gap in a designated section
  Do NOT treat the output as incomplete — a flagged null satisfies
  the completeness requirement; an invented value does not.

  If NO-NEW-SECTIONS is active, flag inline within the field itself
  (e.g. "NOT SPECIFIED — unverifiable") rather than creating a
  designated section.
```

This is the general form of every guardrail you have already seen in the case studies — invented owners, invented dates, invented scores. The completeness deadlock is the same failure pattern, named once so you recognize it everywhere it appears.

A second seam worth knowing by name, specific to Decomposer/Planner programs: the **goal-preservation deadlock**. `SUBGOAL-VERIFY` can fail honestly — a subgoal genuinely cannot be confirmed against the parent goal — at the same moment `MAX-DEPTH` is reached. Depth pressure and verification failure arrive together, and it is tempting to let reaching the depth ceiling stand in for a passed check. It does not. These are independent conditions: one says how far you have recursed, the other says whether what you recursed into is still true.

The resolution rule is the same one that resolved the completeness deadlock: **Layer 3 always wins.** A Decomposer must halt the branch and report it unresolved rather than executing an unverified subgoal to satisfy depth or completeness pressure. State this explicitly in any Decomposer/Planner program:

```
PRIORITY RULE:
IF SUBGOAL-VERIFY fails AND MAX-DEPTH is reached on the same branch:
  Do NOT execute the unverified subgoal.
  Halt the branch.
  Report it as "UNRESOLVED — verification failed at max depth"
  in a designated section, not as a completed step.
  Do NOT treat MAX-DEPTH as satisfying SUBGOAL-VERIFY — reaching
  a depth ceiling is not evidence the subgoal is correct.
```

This is the planning-layer twin of the completeness deadlock: different rules, different layer, same resolution. Once you have seen one named seam resolved this way, you know how to resolve the next one — Layer 3 always wins, and a halted-and-flagged branch is the honest output, not a failed one.

**Before you deploy — four checks.** Run these once before relying on a new program in production.

1. **Memory check.** Some platforms carry memory across sessions or chats. If your program assumes `CHAT-LOCAL` — that nothing outside this conversation should influence the output — verify that assumption holds. Ask the model directly, in a fresh session, what it already knows about you or the topic before you provide any context. If it knows something it should not, the platform has memory you need to account for, and `CHAT-LOCAL` may not behave the way you expect.

2. **Clock check.** Confirm the model reads the actual current date and time when asked, rather than estimating, refusing, or claiming it cannot. If telemetry or `ANCHOR-DATE` depend on an accurate clock read, verify it before building the program around it.

3. **Adversarial input check.** Run one deliberately incomplete or ambiguous input through the program. Confirm it triggers the FAIL-SAFE posture — a flag, a null, a stop — rather than improvising a plausible answer.

4. **Consistency check.** For any Monitor/Evaluator program, submit the same input twice in separate sessions. Confirm the evaluation is identical. If it is not, the standard has drifted before you have even deployed it.

When the eight passes feel instinctive rather than deliberate — when you spot the missing guardrail before writing it, name the seam before it appears, and reach for the right operator without consulting a list — you have internalized the discipline. At that point, the passes are not a process. They are how you think.

**The blank canvas.** If you want to build from scratch rather than adapting a case study, here is the bare frame:

```
STRUCTURE: STRUCTURE-LOCKED
NO-NEW-SECTIONS

SCOPE:
SCOPE: ONLY [what you want]
OUT OF SCOPE: [what you do not want]

RULES (HARD):
MUST: [requirements]
NEVER: [prohibitions]
DO NOT: [scoped prohibitions]

STATE:
DEFINE [variables]
CONSTANT [invariants]

EVIDENCE:
SOURCE-BOUND
CITE YOUR SOURCE
IF NOT IN SOURCE → "NOT SPECIFIED"

GUARDRAILS (CRITICAL):
GUARDRAIL (CRITICAL): [failure mode] → [rule]

DECISION ORDER:
PREFERRED: [condition] → [action]
FALLBACK: [condition] → [action]
LAST RESORT: [condition] → [action]

FLOW:
EXECUTE IN ORDER
FOR EACH [item] IN [set]:
  IF [condition] → [action]
  ELSE → [action]

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)
FAIL-SAFE: IGNORE-ON-AMBIGUITY
IF ambiguous → DO NOT USE VALUE

ACCOUNTABILITY:
DERIVATION MUST BE EXPLAINED
IF INFERRED, STATE METHOD

TELEMETRY:
TURN: [N]
TIMESTAMP: [date/time]
SESSION-ID: [identifier]
ASSUMPTION: [key premise]
```

---

## Part 8 — The Compass and the French Curve

There are two instruments in a drafter's toolkit that do different kinds of work.

The **compass** is the precision instrument. You set the fixed point, set the width, and draw — exactly, every time, regardless of who holds it. It does not decide what to build. It ensures that whatever is built is exact.

The **french curve** is the judgment instrument. It is a template of arcs and curves that have no single formula — the shapes that require feel, proportion, and context. You hold it against the paper, find the section that fits, and trace. The curve is the accumulated wisdom of good shapes, made portable.

Together, these two instruments describe what this guide becomes when used at its full capability.

Here is the part worth pausing on, stated plainly: you do not have to write a governed prompt program by hand. You can have a language model write it for you, using this guide as the specification. Upload this document into a session and tell it: "Use the Compass." The model reads the same taxonomy you just read — the six types, the four layers, the named failure modes, the operator vocabulary — and uses it as its own instructions for how to build something new. You describe what you want in plain language. It identifies the type, applies the layers, selects the operators, writes the guardrails, adds telemetry, and hands you back a finished, governed program. This is the actual capability this guide unlocks: not just words for you to use by hand, but a specification complete enough that the model can use it too.

The six case studies in Part 6 are the french curves — accumulated, proven shapes for common problems. You do not have to design from first principles each time. You hold the template against your situation, find the section that fits, and adapt.

The Compass Prompt in Appendix C is the precision instrument. You describe what you want to build. The model uses this guide's taxonomy to draft a governed program exactly — identifying the type, applying the layers, selecting the operators, writing the guardrails, adding telemetry.

**Here is how to use them together — as simple as 1, 2, 3:**

**Step 1 — Describe your program in plain language.**

Write a paragraph about what you want the program to do. Who uses it? What does it take as input? What does it produce? What must it never do? What has gone wrong in similar work before? You do not need to know which operators to use — that is the model's job. You need to know what you are trying to build and why.

*Example: "I need a program that reviews vendor invoices as they come in, checks them against our purchase orders, flags any discrepancies above $500, and produces a clear summary for the accounts payable team. It must never approve payment — it only flags and routes. I am worried about it inventing matches that are not there."*

**Step 2 — Load the guide and invoke the Compass.**

Upload this guide to your LLM session, or paste in the relevant sections. Then simply say: **"Use the Compass."**

The model will walk through a short intake — asking a few clarifying questions to identify the program type and surface any constraints you did not mention. You answer. It drafts.

**Step 3 — Review the draft, adapt the templates, and make it yours.**

The model will produce a complete, governed prompt program with a "Make it yours" section at the end. Review the guardrails — they should match the failure modes you named. Check the type identification — does the program carry state, enforce a sequence, evaluate against a standard? Adapt the case study template closest to your situation if you want a proven starting shape rather than a blank-canvas draft. Run the program once on a test input before deploying.

The compass gives you precision. The french curve gives you a proven shape. Together, they turn a paragraph description into a governed program — without requiring you to memorize every operator in this guide.

---

## Part 9 — What You Now Have

This guide began with a punchline about code. It ends with something more useful — a practice.

The language model is not a search engine or an assistant in the way those terms are usually understood. It generates responses rather than retrieving them. What it produces is always the result of a program execution. The question has always been: did you write the program, or did the model write it for you?

When you write it — with a declared output contract, locked decision logic, an explicit uncertainty posture, and the right operators in the right places — you get outputs you can trust, act on, and hand to someone else. When you do not, you get outputs that look the same but may be wrong in ways that do not announce themselves.

None of this requires a background in programming. The vocabulary is plain English. The logic is the same logic you practiced in geometry proofs — if this is true, and this is true, then this must follow — applied now to the work you actually do.

And at the advanced level, the guide itself becomes the instrument. The practitioner who understands the system can describe what they want, load the compass, and watch something almost magical come out of the session. Not because the model is guessing. Because you told it exactly what to build — and it knew exactly how.

The discipline is the syntax. The syntax is available to anyone willing to use it.

The punchline at the start of this guide said precision cannot be separated from programming. What the guide has been building toward, in every layer and every pass, is the practical version of that claim: a governed program does not just produce output — it produces output you can act on, hand to someone else, and defend when questioned. Not because the model has become more careful, but because the program was written to make the gaps explicit and the reasoning visible. Fluent output and governed output can look identical. The difference only appears when something goes wrong — and a governed program is the one designed to make that visible before it costs you.

You have the compass and the french curve both. Use them.

---

## Appendix A — Further Reading

Each of these works explores a different dimension of the same underlying idea: that language model behavior is programmable, and that programming it well requires discipline.

**"Prompting Is Programming: A Query Language for Large Language Models"**
Beurer-Kellner et al. (2022). The foundational academic paper establishing that prompts are programs in the formal sense. Introduces LMQL, a query language for LLMs built on constraint-based control flow. If you want the rigorous theoretical underpinning for everything this guide claims practically, start here.
*URL: https://arxiv.org/abs/2212.06094*

**"LLMs Are Stochastic Compilers"**
Aman Madaan (2023). A practitioner-friendly blog post framing language models as compilers that take natural language as input and produce structured behavior as output. A useful bridge between the academic framing and the working practitioner's experience.
*URL: https://madaan.github.io/prompting/*

**"Promptware Engineering: Software Engineering for Prompt-Enabled Systems"**
Chen et al. (2025). An academic paper proposing that prompt development deserves the same systematic discipline as software engineering — requirements, design, testing, debugging, evolution. Names the "promptware crisis" (ad hoc, trial-and-error prompt development) and proposes a structured methodology to address it. The formal research counterpart to the practical approach in this guide.
*URL: https://arxiv.org/abs/2503.02400*

---

## Appendix B — Operator Quick Reference (Alphabetical)

| Operator / Word | Category | What It Does | Good Pairings |
|---|---|---|---|
| `ALWAYS` | Enforcement | Forces a behavior without exception across every output | `MUST` · `STRUCTURE-LOCKED` · `COMPLETE-NOT-PARTIAL` |
| `ANCHOR-DATE: [date]` | Source Control | Declares a fixed reference point for resolving relative time language ("last week," "next quarter") inside the program's reasoning | `TELEMETRY-ON` · `WEB-GROUNDED` · `KNOWLEDGE-CUTOFF-AWARE` |
| `ASK ONLY THE MINIMUM CLARIFYING QUESTIONS REQUIRED` | Behavior Control | Limits interruptions to genuine blockers; prevents stalling | `IF REQUIRED INFO IS MISSING → STOP` · `VERIFY-BEFORE-CONCLUDE` · `ERROR-MODE: FAIL-SAFE` |
| `ASSUMPTIONS-EXPLICIT` | Show Your Work | Every assumption must appear before the conclusion that depends on it | `IF INFERRED, STATE METHOD` · `PROVENANCE-REQUIRED` · `UNCERTAINTY-BOUNDED` |
| `ASSUMPTIONS-EXPLICIT: MAX 3 ASSUMPTIONS` | Show Your Work | Caps permitted assumptions per output; requires stop-and-ask when exceeded | `ASSUMPTIONS-EXPLICIT` · `VERIFY-BEFORE-CONCLUDE` · `ERROR-MODE: FAIL-SAFE` |
| `BOOTLOADER` | Structure — Interpreter | Initialization block; must execute before all other session logic | `DEFINE` · `CONSTANT` · `EXECUTE IN ORDER` |
| `CASE MUST MATCH EXACTLY` | Precision | Prevents normalization of capitalization, acronyms, or proper names | `EXACT MATCH REQUIRED` · `DO NOT NORMALIZE` · `SOURCE-BOUND` |
| `CHAT-LOCAL` | Show Your Work | Constrains all outputs to evidence in this session only | `EMIT-ONLY-DERIVABLE` · `NO-HEURISTICS` · `SOURCE-BOUND` |
| `CHAT-LOCAL: EMIT-ONLY-DERIVABLE` | Show Your Work | Claims must be step-by-step derivable from visible session content | `CHAT-LOCAL` · `PROVENANCE-REQUIRED` · `QUOTE-OR-OMIT` |
| `CHECKPOINT` | Structure — Interpreter | Saves current session state at a defined interval; enables resumption | `DEFINE` · `SET` · `BOOTLOADER` |
| `CITE YOUR SOURCE` | Evidence | Requires attribution of claims to named inputs | `SOURCE-BOUND` · `IF NOT IN SOURCE → "NOT SPECIFIED"` · `DERIVATION MUST BE EXPLAINED` |
| `CLAIM-REQUIRES-ANCHOR` | Show Your Work | Every assertion must be pinned to a specific source location | `PROVENANCE-REQUIRED` · `CITE YOUR SOURCE` · `EMIT-ONLY-DERIVABLE` |
| `COMPLETE-NOT-PARTIAL` | Structure | All defined sections and fields must be populated before output is complete | `STRUCTURE-LOCKED` · `NO-NEW-SECTIONS` · `PROCESS ALL — NO SKIP` |
| `CONFIDENCE-DECLARED` | Show Your Work | Claims not directly quoted from source must carry an explicit confidence label | `ASSUMPTIONS-EXPLICIT` · `UNCERTAINTY-BOUNDED` · `PROVENANCE-REQUIRED` |
| `CONSISTENCY RULE` | Structure — Monitor | Names the signal that reveals standard drift: same input twice must produce same evaluation | `CONSTANT` · `GUARDRAIL (CRITICAL)` · `ERROR-MODE: FAIL-SAFE` |
| `CONSTANT` | State | Declares a value that must not change during execution | `DEFINE` · `SET` · `EXECUTE IN ORDER` |
| `CONTEXT-INHERIT` | Composition | Child program inherits the parent program's evidence and constraints when invoked | `INVOKE` · `SOURCE-BOUND` · `CONTEXT-ISOLATE` |
| `CONTEXT-ISOLATE` | Composition | Child program runs in a fresh frame, with no inherited evidence or constraints from the parent | `INVOKE` · `CONTEXT-INHERIT` · `RETURN` |
| `DECOMPOSITION-BOUND` | Structure — Decomposer | Requires every subgoal to trace explicitly to the parent goal before it may execute | `GOAL-PRESERVATION-CHECK` · `SUBGOAL-VERIFY` · `MAX-DEPTH` |
| `DEFINE` | State | Declares a named variable and its initial value or type | `SET` · `CONSTANT` · `DERIVE` |
| `DEPLOYMENT CONSTRAINT` | Structure — Specification | Specifies how an artifact behaves when loaded into a live session; prevents authority expansion | `GUARDRAIL (CRITICAL)` · `NEVER` · `STRUCTURE-LOCKED` |
| `DERIVATION MUST BE EXPLAINED` | Accountability | Requires the model to show reasoning for conclusions not stated directly in source | `IF INFERRED, STATE METHOD` · `SOURCE-BOUND` · `CITE YOUR SOURCE` |
| `DERIVE` | State | Instructs the model to compute a value from defined inputs rather than assume it | `DEFINE` · `DERIVATION MUST BE EXPLAINED` · `IF INFERRED, STATE METHOD` |
| `DO NOT` | Enforcement | Prohibits a specific behavior in a scoped context; narrower than `NEVER` | `NEVER` · `GUARDRAIL (CRITICAL)` · `OUT OF SCOPE` |
| `DO NOT INVENT VALUES` | Precision | Prevents fabrication of field values when source data is absent | `MUST BE NULL UNLESS` · `IF NOT IN SOURCE → "NOT SPECIFIED"` · `ERROR-MODE: FAIL-SAFE` |
| `DO NOT NORMALIZE` | Precision | Prevents silent standardization or reformatting of values from source | `EXACT MATCH REQUIRED` · `CASE MUST MATCH EXACTLY` · `SOURCE-BOUND` |
| `DO NOT POPULATE UNLESS VALID` | Controlled Emptiness | Prevents any value from appearing unless a defined validity condition is satisfied | `MUST BE NULL UNLESS` · `DO NOT INVENT VALUES` · `FAIL-SAFE: IGNORE-ON-AMBIGUITY` |
| `DO NOT SKIP STEPS` | Flow Control | Enforces sequential execution; prevents jumping to conclusions prematurely | `EXECUTE IN ORDER` · `PROCESS ALL — NO SKIP` · `FOR EACH` |
| `DRIFT-CHECK: EVERY [N] TURNS` | Telemetry | Periodic self-check comparing current output posture to original program intent | `TELEMETRY-ON` · `SESSION-ID` · `ASSUMPTION` |
| `EDGE CASE` | Structure — Interpreter | Names a predictable off-script situation and defines the rule for handling it | `GUARDRAIL (CRITICAL)` · `NEVER` · `ERROR-MODE: FAIL-SAFE` |
| `ELSE` / `ELSE IF` | Flow Control | Defines what the model does when a primary condition is not met | `IF / THEN` · `ONLY IF` · `FOR EACH` |
| `EMIT-ONLY-DERIVABLE` | Show Your Work | If a claim cannot be proven from current context, it must not be stated | `CHAT-LOCAL` · `QUOTE-OR-OMIT` · `NO-GUESSES` |
| `EMIT-ONLY-DERIVABLE: NO-ESTIMATES` | Show Your Work | Closes the approximation loophole; prohibits "~", "about," "roughly" | `EMIT-ONLY-DERIVABLE` · `NO-HEURISTICS` · `HEDGES-ARE-FAILURES` |
| `ERROR-MODE: FAIL-SAFE (NOT FAIL-OPEN)` | Behavior Control | Sets the program's global uncertainty posture; stops or returns null on ambiguity | `FAIL-SAFE: IGNORE-ON-AMBIGUITY` · `IF ambiguous → DO NOT USE VALUE` · `GUARDRAIL (CRITICAL)` |
| `EXACT MATCH REQUIRED` | Precision | Output must reproduce a value exactly as it appears in source | `CASE MUST MATCH EXACTLY` · `DO NOT NORMALIZE` · `SOURCE-BOUND` |
| `EXECUTE IN ORDER` | Flow Control | Forces sequential execution; prevents reordering, parallelizing, or skipping | `STEP 1 → STEP N` · `DO NOT SKIP STEPS` · `FOR EACH` |
| `EXCLUSION LIST` | Behavior Control | Explicitly names items the model must not include, reference, or act on | `OUT OF SCOPE` · `NEVER` · `SCOPE: ONLY` |
| `FAIL-SAFE: IGNORE-ON-AMBIGUITY` | Behavior Control | Excludes an ambiguous value rather than guessing | `ERROR-MODE: FAIL-SAFE` · `IF ambiguous → DO NOT USE VALUE` · `MUST BE NULL UNLESS` |
| `FALLBACK` | Decision Hierarchy | Second-priority action when the preferred condition is not met | `PREFERRED` · `LAST RESORT` · `IF / THEN / ELSE` |
| `FIELD-CONSTRAINED` | Structure | Restricts output to named fields only; prevents unrequested additions | `STRUCTURE-LOCKED` · `NO-NEW-SECTIONS` · `COMPLETE-NOT-PARTIAL` |
| `FOR EACH` | Flow Control | Iterates over a defined set and applies the same logic to every item | `PROCESS ALL — NO SKIP` · `EXECUTE IN ORDER` · `DO NOT SKIP STEPS` |
| `GOAL-PRESERVATION-CHECK` | Structure — Decomposer | After executing a subgoal, restates the parent goal and confirms the subgoal's output still serves it | `DECOMPOSITION-BOUND` · `SUBGOAL-VERIFY` · `GUARDRAIL (CRITICAL)` |
| `GUARDRAIL (CRITICAL): [name] → [rule]` | Decision Control | Names a known failure mode and prohibits it explicitly; the most powerful second-order operator | `ERROR-MODE: FAIL-SAFE` · `NEVER` · `DO NOT INVENT VALUES` |
| `HEDGES-ARE-FAILURES` | Show Your Work | Hedging language in factual outputs is a generation error requiring correction | `NO-GUESSES` · `EMIT-ONLY-DERIVABLE` · `ERROR-MODE: FAIL-SAFE` |
| `IF / THEN` | Flow Control | Defines conditional execution; model acts only when stated condition is true | `ELSE` · `ONLY IF` · `FOR EACH` |
| `IF ambiguous → DO NOT USE VALUE` | Behavior Control | Explicit instruction for ambiguous values; closes the most common Layer 3 gap | `FAIL-SAFE: IGNORE-ON-AMBIGUITY` · `ERROR-MODE: FAIL-SAFE` · `MUST BE NULL UNLESS` |
| `IF INFERRED, STATE METHOD` | Accountability | Requires the model to label inferences and explain how they were derived | `DERIVATION MUST BE EXPLAINED` · `CITE YOUR SOURCE` · `ASSUMPTIONS-EXPLICIT` |
| `IF NOT IN SOURCE → "NOT SPECIFIED"` | Evidence | Forces explicit missingness; prevents bridging gaps with invention | `SOURCE-BOUND` · `CITE YOUR SOURCE` · `DO NOT INVENT VALUES` |
| `IF REQUIRED INFO IS MISSING → STOP AND LIST MISSING INPUTS` | Behavior Control | Hard stop on insufficient input; prevents proceeding on a partial brief | `ASK ONLY THE MINIMUM` · `ERROR-MODE: FAIL-SAFE` · `COMPLETE-NOT-PARTIAL` |
| `INVOKE: [program-name]` | Composition | Calls another prompt program as a function; the child runs and returns to the parent | `CONTEXT-INHERIT` · `CONTEXT-ISOLATE` · `RETURN` |
| `KNOWLEDGE-CUTOFF-AWARE` | Source Control | Flags that the output should note where training data age is a known limitation | `WEB-GROUNDED` · `SEARCH-REQUIRED` · `ASSUMPTIONS-EXPLICIT` |
| `LAST RESORT` | Decision Hierarchy | Final fallback when preferred and fallback conditions are both unmet | `PREFERRED` · `FALLBACK` · `ERROR-MODE: FAIL-SAFE` |
| `LITERAL-ONLY` | Show Your Work | Disables metaphor, analogy, interpretive framing; output must be literal | `EMIT-ONLY-DERIVABLE` · `SOURCE-BOUND` · `NO-INFERENTIAL-BRIDGE` |
| `MAX-DEPTH: [N]` | Structure — Decomposer | Hard ceiling on recursion depth for a Decomposer/Planner program | `DECOMPOSITION-BOUND` · `SUBGOAL-VERIFY` · `GUARDRAIL (CRITICAL)` |
| `MAX ITERATIONS` | Flow Control | Hard ceiling on loop execution; prevents runaway iteration | `FOR EACH` · `STOP WHEN` · `REPEAT · UNTIL` |
| `MUST` | Enforcement | Non-negotiable requirement; output is incomplete if not satisfied | `ALWAYS` · `STRUCTURE-LOCKED` · `COMPLETE-NOT-PARTIAL` |
| `MUST BE NULL UNLESS [condition]` | Controlled Emptiness | Field must remain empty unless an explicit condition is satisfied | `DO NOT POPULATE UNLESS VALID` · `DO NOT INVENT VALUES` · `FAIL-SAFE: IGNORE-ON-AMBIGUITY` |
| `NEVER` | Enforcement | Global prohibition; applies across the entire program with no exceptions | `DO NOT` · `GUARDRAIL (CRITICAL)` · `OUT OF SCOPE` |
| `NO-GUESSES` | Show Your Work | Missing facts must be stated as missing; inference is not a substitute | `NO-GUESSES: SAY "UNKNOWN"` · `EMIT-ONLY-DERIVABLE` · `ERROR-MODE: FAIL-SAFE` |
| `NO-GUESSES: SAY "UNKNOWN"` | Show Your Work | "Might," "likely," "probably" must become "UNKNOWN" | `NO-GUESSES` · `HEDGES-ARE-FAILURES` · `UNCERTAINTY-BOUNDED: DEFAULT UNKNOWN` |
| `NO-HEURISTICS` | Show Your Work | Disables approximation: estimates, implied continuity, assumed counters | `NO-HEURISTICS: NUMBERS MUST BE COUNTED` · `EMIT-ONLY-DERIVABLE` · `CHAT-LOCAL` |
| `NO-HEURISTICS: NUMBERS MUST BE COUNTED` | Show Your Work | Numeric outputs must be counted or derived; approximation markers are errors | `NO-HEURISTICS` · `EXACT MATCH REQUIRED` · `HEDGES-ARE-FAILURES` |
| `NO-INFERENTIAL-BRIDGE` | Show Your Work | Prevents connecting two facts with a causal claim not stated in source | `SOURCE-BOUND` · `PROVENANCE-REQUIRED` · `CLAIM-REQUIRES-ANCHOR` |
| `NO-NEW-SECTIONS` | Structure | Prevents the model from adding sections beyond the defined output contract | `STRUCTURE-LOCKED` · `FIELD-CONSTRAINED` · `COMPLETE-NOT-PARTIAL` |
| `NO REWRITES` | Scope | Prevents paraphrasing or restructuring of source content | `NO REPHRASE` · `SOURCE-BOUND` · `EXACT MATCH REQUIRED` |
| `NO REPHRASE` | Scope | Prevents synonym substitution or stylistic reformatting | `NO REWRITES` · `DO NOT NORMALIZE` · `SOURCE-BOUND` |
| `ONLY` | Enforcement | Restricts the model to a single permitted action, source, or output type | `SCOPE: ONLY` · `MUST` · `DO NOT` |
| `ONLY IF` | Flow Control | Stronger conditional than `IF`; action may not occur under any other condition | `IF / THEN` · `GUARDRAIL (CRITICAL)` · `NEVER` |
| `OUT OF SCOPE: [Y]` | Scope | Explicitly names what the model must not include or reason from | `SCOPE: ONLY` · `NEVER` · `EXCLUSION LIST` |
| `PARTICIPATION BOUNDARY` | Structure — Specification | Names what the artifact may never do; closes authority creep before it starts | `GUARDRAIL (CRITICAL)` · `NEVER` · `DEPLOYMENT CONSTRAINT` |
| `PHASE GATE` | Structure — Process | Transition condition between phases; blocks advancement until phase is confirmed complete | `VERIFY-BEFORE-CONCLUDE` · `STOP` · `EXECUTE IN ORDER` |
| `PREFERRED` | Decision Hierarchy | First-priority action when multiple options could apply | `FALLBACK` · `LAST RESORT` · `IF / THEN / ELSE` |
| `PROCESS ALL — NO SKIP` | Flow Control | Requires complete iteration with no items omitted | `FOR EACH` · `EXECUTE IN ORDER` · `COMPLETE-NOT-PARTIAL` |
| `PROVENANCE-REQUIRED` | Show Your Work | Every claim must be traceable to user text, document, or explicit assumption | `CITE YOUR SOURCE` · `CLAIM-REQUIRES-ANCHOR` · `QUOTE-OR-OMIT` |
| `PROVENANCE-REQUIRED: QUOTE-OR-OMIT` | Show Your Work | Unsupported claims must be omitted, not hedged | `QUOTE-OR-OMIT` · `EMIT-ONLY-DERIVABLE` · `SOURCE-BOUND` |
| `QUOTE-OR-OMIT` | Show Your Work | Facts not quotable from the session or document do not appear in output | `PROVENANCE-REQUIRED` · `EMIT-ONLY-DERIVABLE` · `CLAIM-REQUIRES-ANCHOR` |
| `QUOTE-OR-OMIT: DIRECT-QUOTE ONLY` | Show Your Work | Paraphrases presented as quotes are violations; only exact source reproduction permitted | `QUOTE-OR-OMIT` · `EXACT MATCH REQUIRED` · `NO REWRITES` |
| `REPEAT · UNTIL` | Flow Control | Loop that continues until a condition is met; must pair with termination condition | `MAX ITERATIONS` · `STOP WHEN` · `FOR EACH` |
| `RETRIEVAL-BOUND` | Source Control | Constrains the model to chunks actually returned by retrieval this turn — not training knowledge, not prior sessions, not what "should have" been retrieved | `CHAT-LOCAL` · `PROVENANCE-REQUIRED` · `GUARDRAIL (CRITICAL): RETRIEVAL-SILENT-MISS` |
| `RETURN: [shape]` | Composition | Declares the output contract a child program must satisfy when returning to its parent | `INVOKE` · `STRUCTURE-LOCKED` · `COMPLETE-NOT-PARTIAL` |
| `SCOPE: ONLY [X]` | Scope | Declares exclusive permitted scope; everything outside is implicitly excluded | `OUT OF SCOPE` · `EXCLUSION LIST` · `SOURCE-BOUND` |
| `SEARCH-REQUIRED: [topic]` | Source Control | Names a specific area where training data is insufficient and live search is required | `WEB-GROUNDED` · `KNOWLEDGE-CUTOFF-AWARE` · `SOURCE-BOUND` |
| `SESSION-ID: [identifier]` | Telemetry | Names the session for later reference and reseating across sessions | `TELEMETRY-ON` · `TURN` · `DRIFT-CHECK` |
| `SESSION RULES (NON-NEGOTIABLE)` | Structure | Declares program-level rules that apply throughout without exception | `CONSTANT` · `NEVER` · `GUARDRAIL (CRITICAL)` |
| `SET` | State | Assigns a value to a defined variable; updates the state register | `DEFINE` · `CONSTANT` · `DERIVE` |
| `SOURCE-BOUND` | Evidence | Restricts all claims, derivations, and outputs to named inputs | `CITE YOUR SOURCE` · `IF NOT IN SOURCE → "NOT SPECIFIED"` · `DO NOT INVENT VALUES` |
| `STANDARD DECLARATION` | Structure — Monitor | Declares the fixed evaluation standard before any content is reviewed | `CONSTANT` · `GUARDRAIL (CRITICAL): STANDARD DRIFT` · `DO NOT update` |
| `STEP 1 → … → STEP N` | Flow Control | Explicit numbered execution sequence; strongest ordering constraint | `EXECUTE IN ORDER` · `DO NOT SKIP STEPS` · `COMPLETE-NOT-PARTIAL` |
| `STOP WHEN` | Flow Control | Defines the termination condition for a loop or session | `MAX ITERATIONS` · `REPEAT · UNTIL` · `COMPLETE-NOT-PARTIAL` |
| `STRUCTURE-LOCKED` | Structure | Freezes the output schema; model may not add, remove, or reorder sections | `NO-NEW-SECTIONS` · `COMPLETE-NOT-PARTIAL` · `FIELD-CONSTRAINED` |
| `SUBGOAL-VERIFY` | Structure — Decomposer | Before executing a subgoal, confirms it still serves the parent goal; discard and re-decompose rather than patch on failure | `DECOMPOSITION-BOUND` · `GOAL-PRESERVATION-CHECK` · `MAX-DEPTH` |
| `TELEMETRY-ON` | Telemetry | Activates a minimal footer on every output: turn count, timestamp, key assumption | `TURN` · `TIMESTAMP` · `SESSION-ID` · `ASSUMPTION` |
| `TIMESTAMP: [format]` | Telemetry | Requires a clock read from the model on each output. Models should always read the clock rather than estimate. | `TELEMETRY-ON` · `TURN` · `SESSION-ID` |
| `TRAINING-ONLY` | Source Control | Explicitly prohibits web search; model draws only from training and session content | `SCOPE: ONLY` · `CHAT-LOCAL` · `ERROR-MODE: FAIL-SAFE` |
| `TRIGGER` | Structure — State Machine | Defines the specific input that fires the program's primary execution sequence | `IF / THEN` · `EXECUTE IN ORDER` · `DO NOT generate until` |
| `TURN: [N]` | Telemetry | Explicit turn counter embedded in the output; increments with every response | `TELEMETRY-ON` · `TIMESTAMP` · `DRIFT-CHECK` |
| `UNCERTAINTY-BOUNDED` | Show Your Work | Uncertainty must be a bounded range or "unknown"; vagueness and false precision are both failures | `UNCERTAINTY-BOUNDED: DEFAULT UNKNOWN` · `ASSUMPTIONS-EXPLICIT` · `NO-GUESSES` |
| `UNCERTAINTY-BOUNDED: DEFAULT UNKNOWN` | Show Your Work | When a range cannot be derived from evidence, defaults to "unknown" | `UNCERTAINTY-BOUNDED` · `NO-GUESSES: SAY "UNKNOWN"` · `ERROR-MODE: FAIL-SAFE` |
| `VERIFY-BEFORE-CONCLUDE` | Show Your Work | Missing information must surface before a conclusion is generated | `VERIFY-BEFORE-CONCLUDE: ASK 1-2 QUESTIONS MAX` · `ASK ONLY THE MINIMUM` · `ERROR-MODE: FAIL-SAFE` |
| `VERIFY-BEFORE-CONCLUDE: ASK 1-2 QUESTIONS MAX` | Show Your Work | Limits verification to minimum blocking questions | `VERIFY-BEFORE-CONCLUDE` · `ASK ONLY THE MINIMUM` · `IF REQUIRED INFO IS MISSING → STOP` |
| `WEB-GROUNDED` | Source Control | Signals that the model should search for current information beyond training data | `SEARCH-REQUIRED` · `KNOWLEDGE-CUTOFF-AWARE` · `CITE YOUR SOURCE` |

---

## Appendix C — The Compass

*This appendix is a self-contained prompt program. To use it: upload this guide to your LLM session, then say "Use the Compass." The program will run the intake below and produce a governed prompt program based on your description.*

---

```
COMPASS — LLM MAGIC WORDS PROGRAM BUILDER
Activated by: "Use the Compass"

BOOTLOADER — EXECUTE BEFORE ALL ELSE:
You have been given the LLM Magic Words guide as your complete reference.
Use it as the specification for what a well-built prompt program looks like.
Do not begin drafting until the intake below is complete.

SESSION RULES (NON-NEGOTIABLE):
ONE QUESTION AT A TIME. Ask, then STOP. Wait for the answer.
Do not identify the program type until Q4 is answered.
Do not begin drafting until the intake is complete.
ASSUMPTIONS-EXPLICIT: MAX 3 ASSUMPTIONS
If the description is too ambiguous to draft after Q4, ask one clarifying
question before proceeding. If more than three assumptions are required
to begin drafting, stop and list what is missing.

---

INTAKE — EXECUTE IN ORDER:

Q1: "What do you want this program to do? Describe it in plain language —
     who uses it, what it takes as input, what it produces as output."

Q2: "What must this program never do? Name the boundaries — the outputs
     it must not produce, the actions it must not take, the mistakes
     you are trying to prevent."

Q3: "Is there anything time-sensitive or current in what this program
     needs to know? Or can it work entirely from what you give it
     in the session?"

Q4: "How will this program be used — once by you, repeatedly by your
     team, loaded by others into different sessions, or as a recurring
     session that picks up where it left off?"

---

AFTER INTAKE — EXECUTE IN ORDER:

STEP 1 — NAME THE TYPE:
Based on the answers, identify which of the six program types applies:
State Machine / Interpreter / Process Definition /
Specification Language / Monitor-Evaluator / Decomposer-Planner
If it shares features of more than one, name the primary and secondary.
If the description involves breaking a goal into subgoals or recursive
planning, Decomposer/Planner is likely primary even if it borrows
structure from another type.
State your reasoning in one sentence before continuing.

STEP 2 — NAME THE FAILURE MODES:
Based on the type and the intake answers, name the specific failure modes
this program must prevent. Start from the guide's per-type failure modes.
Add any that emerged from the intake.

STEP 3 — DRAFT THE PROGRAM:
Using the guide's operator vocabulary and the blank canvas from Part 7,
draft the complete governed prompt program.
Structure it for the identified type:
- State Machine: include TRIGGER, state-read rule, TELEMETRY block
- Interpreter: include BOOTLOADER, named variables, CHECKPOINT, EDGE CASE handlers
- Process Definition: include PHASE GATES, VERIFY-BEFORE-CONCLUDE at each gate
- Specification Language: include TYPE CONSTRAINT, PARTICIPATION BOUNDARY,
  DEPLOYMENT CONSTRAINT, MAINTENANCE section
- Monitor/Evaluator: include STANDARD DECLARATION as CONSTANT,
  DO NOT update rule, CONSISTENCY RULE, TELEMETRY block
- Decomposer/Planner: include CONSTANT parent_goal, MAX-DEPTH,
  DECOMPOSITION-BOUND, SUBGOAL-VERIFY before each subgoal,
  GOAL-PRESERVATION-CHECK after each subgoal, TELEMETRY block with DEPTH

Add Layer 4 (Show Your Work) operators when you — or anyone else — will
act on the output without re-deriving it yourself.

Add Composition operators (INVOKE, CONTEXT-INHERIT, CONTEXT-ISOLATE, RETURN)
if the intake indicates this program is meant to call, or be called by,
another prompt program rather than run standalone.

Add source control operators (WEB-GROUNDED, SEARCH-REQUIRED) if Q3 indicated
the program needs current information beyond what is in the session.

STEP 4 — PRODUCE "MAKE IT YOURS":
After the program, list five to eight specific lines a user should change
to personalize it for their context. For each line: name it, explain what
it controls, give an example substitution.

STEP 5 — TELEMETRY CHECK:
Confirm the program includes a TELEMETRY block. If not, add one.
Minimum: TURN counter, TIMESTAMP, key ASSUMPTION.

STEP 6 — DEPLOYMENT CHECK:
If the program uses CHAT-LOCAL, remind the user to verify the platform
does not carry memory across sessions in a way that would violate it.
If the program uses TIMESTAMP or ANCHOR-DATE, remind the user to confirm
the model can read the actual current date before relying on it.

UNCERTAINTY:
ERROR-MODE: FAIL-SAFE
If the intake does not contain enough information to write a governed program,
do not produce a partially governed program.
State what is missing and what is needed to proceed.
```

---

*End of guide — Draft v7*
