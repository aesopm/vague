# vague

[한국어](README.ko.md)

A discussion skill for when you only half know what you want. Works in Claude Code and Codex.

The skill judges whether this is code work or analysis/design, and whether to only discuss or to edit files, with a one-line reason for each judgment. You steer by overriding its judgments.

## When to use it

- You want to build something but can't pin down its shape
- You're not even sure whether it's code work or planning/analysis
- You want to set a direction for a root-cause analysis or a system design

## How it works

- The first response breaks your words into **goal / already decided / unknown / your hunch**.
- It judges the **nature** (code generation / analysis·design) and the **stage**:
  - Requirements unclear → asks only the questions that matter most, as choices
  - Only a hunch about the shape → shows 2–3 interpretations as previews
  - You don't know the options → lays out different approaches without recommending
  - Requirements sufficient → offers to wrap up
- It judges only what you're unsure about; what's already clear goes under "already decided". When what you want shifts mid-conversation, it breaks your words down again and re-judges.
- Research depth, action (discuss only / edit files / write a doc), and output are judged when they come up.
- It replies in your language. Until the discussion ends, every response ends with an updated **requirements ledger** (confirmed / candidate / rejected).
- Changes like editing files happen only after the skill presents an action judgment (what changes and where) and you confirm it. Asking back or sketching an idea doesn't count as approval. Reading and research need no confirmation.
- The discussion ends when the confirmed action is done, or when you say to stop.

Overriding a judgment (matched by meaning, in any language):

| Say | What happens |
|---|---|
| "no, that's not it" / "not this" | Presents interpretations again |
| "show me the options first" | Lays out options |
| "you decide" | Decides the open question itself, marks it assumed, and continues. If nothing is left open, goes straight to the action judgment |
| "wrap up" | Requirements → pros/cons table → decision and reasons |
| "stop" / "that's enough" | Ends the discussion |

## Example

A condensed version of a real session where vague was used to rework vague itself.

**User:** `/vague:vague vague currently assumes code work. I'd like a version that works only for analysis, ideas, and planning.`

> - **Goal:** a variant for analysis, ideas, and planning without assuming code work
> - **Already decided:** a "specialized version" separate from the current vague; scope is analysis, ideas, planning
> - **Unknown:** what gets analyzed, how deep to research, where results go, how it relates to the current vague
> - **Hunch:** the current skeleton with the code-specific parts swapped out
>
> **Judgment: requirements unclear.** If "analysis" means code analysis, the research limit has to be lifted; if not, the limit isn't needed at all, so the rules go in opposite directions.
>
> *(4 multiple-choice questions: analysis target / where results go / planning scope / separate skill vs. built in)*

**User:** Don't make it a separate skill, put it inside vague. I might not know which of the two to pick either.

> Recommendation changed: built into vague instead of a separate skill, and vague judges the nature too.
>
> **Judgment: only a hunch about the shape.** Here are candidates A / B / C as previews (no recommendation).
>
> **Requirements ledger**
> - Confirmed: built into vague, research depth judged by topic, …
> - Candidate: approach A / B / C, switching nature mid-conversation (assumed), …
> - Rejected: separate skill

**User:** Whether to edit files or only discuss should also be decided through discussion and judgment.

> One decision is overturned. The fixed "discuss only" rule becomes a judgment, so the skeleton becomes "fixed rules + judgment items".
>
> *(Wrap-up: requirements → pros/cons table → decision and reasons → changed parts)*
>
> **Action judgment:** replace SKILL.md with this draft. Confirm and I'll do it.

**User:** Proceed.

## Install

### Claude Code

```
/plugin marketplace add aesopm/vague
/plugin install vague@vague
```

Invoke: `/vague:vague <topic and whatever comes to mind>`

Manual: put `skills/vague/SKILL.md` at `~/.claude/skills/vague/SKILL.md` and invoke with `/vague`.

### Codex

```bash
codex plugin marketplace add aesopm/vague
codex plugin add vague@vague
```

Manual: copy the `skills/vague` folder to `~/.codex/skills/vague` (global) or `.agents/skills/vague` in a repo, and invoke with `$vague`.

`skills/vague/agents/openai.yaml` turns off implicit invocation, so Codex uses the skill only when you call it. In Claude Code, `disable-model-invocation` does the same.

## Evals

`evals/` checks the fixed rules: no changes before confirmation, the first-response breakdown and the ledger, at most 4 questions, empty invocation, reply language, and the overrides that fit in one message ("show me the options first", "you decide", "wrap up"). "Not this" and "stop" need earlier turns, so they aren't covered. Run it from the repo root with Claude Code:

```
claude plugin eval . --allow-tools Write Edit
```

`--allow-tools Write Edit` gives the agent the tools it must not use before confirmation; without it, those checks pass trivially. Each case runs on your own account and is compared against a run without the plugin, so it costs tokens. Add `--runs 1` for a quick pass.

## License

[MIT](LICENSE)
