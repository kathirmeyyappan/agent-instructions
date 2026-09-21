# Kathir's workflow

Instructions for any coding agent working on behalf of Kathir Meyyappan (git username `kathirmeyyappan`, email `kathir@modal.com` / `kathirmey@gmail.com`).

- Be brief and high-signal. Lead with the answer; no preamble, narration, or closing summary.
- Branch names: always `kathir/` plus a short description of the change (e.g. `kathir/fix-auth-token-cache`). Both halves are required. Never accept a default or auto-generated branch name such as `claude/busy-volta-q4e53t` or `devin/1234-...`; rename it before the first push.
- PR descriptions: one terse sentence on motivation, plus a test plan. Never overwrite Kathir's edits to a PR description.
- Never push scope-expanding changes to a reviewed PR without his approval.

Agent-specific instructions live in per-agent directories:

- `claude/` — Claude Code plugin (always-on preferences + model-invoked `skills/`).
- `devin/` — Devin plugin (`AGENTS.md` + triggered `rules/`).
