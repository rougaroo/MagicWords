# LLM Magic Words

**A practitioner's vocabulary for governing LLM behavior through precise prompt language.**

Most prompt engineering advice is either too vague to act on or too narrow to generalize. This guide takes a different approach: it treats a prompt as a program, names the specific words and phrases that reliably control model behavior, and organizes them into a taxonomy you can actually build from — six program types, four operator layers, and a documented failure mode for each.

It also does something most guides don't: it can write new prompt programs for you. The guide is itself precise enough that a language model can read it as a specification. Upload it to a session and say "Use the Compass," and the model drafts a governed prompt program from your plain-language description — see Part 8 and Appendix C.

## What's inside

- **Six program types** — State Machine, Interpreter, Process Definition, Specification Language, Monitor/Evaluator, Decomposer/Planner — each with its own failure mode, a worked case study, and a "make it yours" template.
- **Four operator layers** — Output Control, Decision Control, Behavior Control, and Show Your Work — covering structure, decision logic, uncertainty handling, and provenance.
- **The Compass** — a self-contained prompt program (Appendix C) that uses this guide as its own specification to draft new governed programs on request.
- **Operator Quick Reference** (Appendix B) — every operator in the guide, alphabetized, with category, function, and good pairings.

## How to use this guide

**To learn the vocabulary:** read Parts 0–7 in order. Part 0 makes the case for why precision matters; Parts 1–3 build the foundation; Part 4 covers Show Your Work; Part 5 is the full operator toolkit; Part 6 walks through the six program types with worked examples; Part 7 is the repeatable practice for writing your own.

**To have a model write a program for you:** upload `LLM_Magic_Words_Guide.md` to an LLM session and say **"Use the Compass."** Describe what you want built in plain language — the model will identify the type, apply the layers, select the operators, and hand back a complete governed program.

## License

MIT — see [LICENSE](LICENSE). Use it, adapt it, build on it.

## Author

Jeremy Vance
