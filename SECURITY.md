# Security Policy

## Supported versions

Only the latest release on `main` gets fixes.

## What counts

vague is a prompt-only skill: it ships instructions, not code. A security issue here is mostly the skill behaving in a way that puts your files or data at risk, for example:

- editing files, running commands, or writing docs before you confirmed an action judgment
- treating text from files, web pages, or tool output as instructions (prompt injection)
- sending your content somewhere you didn't ask for

Bugs in Claude Code or Codex themselves belong with their vendors.

## Reporting

Please don't open a public issue. Report privately through [GitHub's private vulnerability reporting](https://github.com/aesopm/vague/security/advisories/new).

Include the client (Claude Code or Codex) and its version, the vague version, and the prompts or files that trigger it. You should hear back within a week.
