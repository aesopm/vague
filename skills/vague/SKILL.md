---
name: vague
description: Discussion for when the user only half knows what they want. Judges the nature, stage, research depth, and action on its own, keeps a requirements ledger, and acts only after the user confirms a judgment.
argument-hint: <topic and whatever thoughts come to mind>
disable-model-invocation: true
---

The user only half knows what they want. You judge almost everything, including whether this is code work or analysis/design, and whether to only discuss or to edit files.

The topic is whatever the user wrote when invoking this skill. If they wrote nothing, ask in one line what's on their mind, with no ledger until there is a topic.

## Fixed rules

- Reply in the user's language, including the ledger and the closing.
- Give a one-line reason for every judgment. If the user overrides it, follow. Match these by meaning, in any language:
  - "no, that's not it" / "not this" → present interpretations again
  - "show me the options first" → lay out options
  - "you decide" → decide the open question yourself, mark it assumed in the ledger, and continue; if nothing is left open, present the action judgment, with no closing offer and no question about an example
  - "wrap up" → closing below, without offering it first
  - "stop" / "that's enough" → end the discussion
- If a new condition changes an earlier judgment or recommendation, say so in one line.
- What the user wrote can differ from what they want, or be wrong. If a stated want, constraint, or premise (including one under "already decided") conflicts with their goal, with what you found, or with the facts, and following it as written would change the outcome, say so in one line with the reason and let them decide. Don't quietly follow it or quietly correct it.
- Never defer a requirement the user mentioned to "when it's needed". This overrides any minimal-implementation bias.
- Until the discussion ends, end every response with an updated **requirements ledger**: confirmed / candidate (mentioned but not confirmed) / rejected. It ends when the action the user confirmed is done, or when they say to stop.
- The user asking back or sketching an idea is not approval. Make changes (edit files, run commands with side effects, publish) only when the user explicitly confirms an action judgment you presented; reading and research need no confirmation. An action judgment says what changes and where, then asks for confirmation. If "proceed" comes before any action judgment, including in the message that invokes this skill ("just do it"), judge the action and present it first; that earlier "proceed" is not confirmation.
- Only if no one can reply right away (a scheduled or CI run with no user in the session, or the conversation is an issue or comment thread; a live chat never counts, even with auto-approved tools or an auto mode), put your questions in one message (still at most 4, each with numbered choices, no extra open questions), end with the ledger, and stop. Don't guess the answers to keep going; pick up from the reply.

## Judgments

In the first response, break the user's words into **goal / already decided / unknown / the user's hunch (shape)**, then judge the nature and the stage. Judge the remaining items when they come up.

Judge them on what the user is unsure about; parts already clear go under "already decided" and into the ledger. What the user wants can shift mid-conversation: a decided part reopens, the goal changes, a new want appears. When it does, break their words down again and re-judge.

**Nature**
- **Code generation** → deciding the shape of code right before implementing
- **Analysis/design** → root-cause analysis, understanding structure, ideas, planning, system design

**Stage**
- **Requirements unclear** (the goal, or a constraint that would change the answer, is missing) → ask at most 4 questions, only those that most affect the decision, as choices (if someone can reply now and a multiple-choice question tool is available, ask through the tool, not as text; otherwise a numbered list; mark the recommendation where one option is better for the goal, not on questions about facts only the user knows; one line per option on how it changes the outcome).
- **Only a hunch about the shape** (the user describes the result by feel or analogy) → show 2–3 interpretations of the user's description as previews and let them pick. Don't push your preferred shape first.
- **Doesn't know what the options are** (the user asks how it's usually done, or the area is new to them) → lay out approaches of different character without recommending (shape / where it fits / where it breaks / requirements it assumes).
- **Requirements sufficient** (no open question left would change the decision) → offer the closing below.

**Remaining items**: start from these defaults and judge.

| | Code generation | Analysis/design |
|---|---|---|
| Preview | Short code | Outline, table, flow. For system design, skeleton code (types, signatures, call flow; no bodies) |
| Research depth | Up to checking types | Read deeply when tracing a cause |
| Scope | Shape of the code | What and why, plus structural how. Don't break it into files or task steps |
| Action | Discuss → implement | Discuss → switch to code generation, or write a document |
| Closing example | Example code | Skeleton code for system design; otherwise a draft of the key part |

Put documents where the project's conventions (CLAUDE.md, memory) say; if there are none, ask.

## Closing

When the requirements seem sufficient, first offer "Shall I lay it all out from the start and decide?", and also ask whether to include an example (the closing example from the defaults table) and whether to end with a handoff (below) for someone else, or an agent, to build from; ask these through a multiple-choice question tool if one is available, as in the stage questions. Don't ask what the user has already decided. If the user agrees, wrap up in this order:

1. Requirements (separate confirmed from assumed; flag assumptions so the user can correct them); with a handoff, skip this and put the handoff last
2. Pros/cons table of the candidates
3. Decision and reasons
4. Example (only if the user chose to include it)
5. Handoff (only if the user chose it)

## Handoff

A handoff adds one condition to "requirements sufficient": every confirmed requirement can be checked, either automatically (say how: a command, a test, an observable state) or by a person (mark it human). Turn vague wording ("feels natural", "fast enough") into a measurable criterion or mark it human; if you can't tell which, ask. If the project's conventions point to criteria for what is checked automatically and what needs a person, use them.

Write it in the user's language, putting each item in exactly one section:

1. Acceptance: what the result must do; auto (each with how it is checked) / human
2. Constraints: limits the user confirmed on how it is built (an interface to keep, a library to use), not behavior
3. Assumptions: everything marked assumed, including what you decided for the user ("you decide"), each with an ID (A1, A2, …), for the reader to confirm before building
4. Non-goals: the rejected items, so the scope doesn't grow back
5. The same content as a JSON block:

```json
{
  "acceptance": {
    "auto": [{ "criterion": "...", "check": "..." }],
    "human": ["..."]
  },
  "constraints": ["..."],
  "assumptions": [{ "id": "A1", "text": "..." }],
  "non_goals": ["..."]
}
```

Show it in the reply. Saving or posting it elsewhere (a file, an issue) is an action: put it where the project's conventions say, otherwise ask.
