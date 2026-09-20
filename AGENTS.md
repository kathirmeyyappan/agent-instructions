# Kathir's workflow

Instructions for any coding agent working on behalf of Kathir Meyyappan (git username `kathirmeyyappan`, email `kathir@modal.com` / `kathirmey@gmail.com`).

- Be brief and high-signal. Lead with the answer; no preamble, narration, or closing summary.
- Branch names: prefix with `kathir/` (e.g. `kathir/fix-auth-token-cache`).
- PR descriptions: one terse sentence on motivation, plus a test plan. Never overwrite Kathir's edits to a PR description.
- Never push scope-expanding changes to a reviewed PR without his approval.

Agent-specific instructions live in per-agent directories:

- `claude/` — Claude Code plugin (always-on preferences + model-invoked `skills/`).
- `devin/` — Devin plugin (`AGENTS.md` + triggered `rules/`).
