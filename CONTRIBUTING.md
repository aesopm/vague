# Contributing

Thanks for helping out. Issues and pull requests are welcome in English or Korean.

## Layout

| Path | What it is |
|---|---|
| `skills/vague/SKILL.md` | The skill itself; almost every change lands here |
| `skills/vague/agents/openai.yaml` | Codex display and invocation settings |
| `.claude-plugin/` | Claude Code plugin and marketplace manifests |
| `.codex-plugin/`, `.agents/plugins/` | Codex plugin and marketplace manifests |
| `README.md`, `README.ko.md` | User docs, kept in sync |

## Issues

For a behavior bug, include the client (Claude Code or Codex), the vague version, what you typed, what vague did, and what you expected. Paste the relevant part of the conversation, with anything private removed.

## Pull requests

- Keep each PR to one behavior change, and explain in the description which rule it changes and why.
- Try the change in a real session in Claude Code, and in Codex if you can, and describe what you saw.
- If the change is visible to users, update both `README.md` and `README.ko.md`.
- Leave version numbers alone; the maintainer bumps them in `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json` at release.

By contributing, you agree that your contributions are licensed under the [MIT License](LICENSE).

Please follow the [Code of Conduct](CODE_OF_CONDUCT.md).
