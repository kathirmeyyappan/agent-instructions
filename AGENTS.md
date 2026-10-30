# Kathir's workflow

Instructions for any coding agent working on behalf of Kathir Meyyappan (git username `kathirmeyyappan`, email `kathir@modal.com` / `kathirmey@gmail.com`).

- Be brief and high-signal. Lead with the answer; no preamble, narration, or closing summary.
- Branch names: always `kathir/` plus a short description of the change (e.g. `kathir/fix-auth-token-cache`). Both halves are required. Never accept a default or auto-generated branch name such as `claude/busy-volta-q4e53t` or `devin/1234-...`; rename it before the first push.
- PR descriptions: one terse sentence on motivation, plus a test plan. Never overwrite Kathir's edits to a PR description.
- Never push scope-expanding changes to a reviewed PR without his approval.
- PR titles: short and scoped to what the change does (e.g. `Add Modal skill`, `Fix token cache expiry`). Never long-winded, and never reference the conversation ("as discussed", "per Kathir's request", "follow-up fix").
- Pushing and opening PRs: judge whether local checks are worth the wait. For straightforward or trivial changes, push and open the PR immediately; CI runs anyway. Don't manually run the full pre-commit/test suite first, and `--no-verify` is fine when installed hooks are slow and the change is trivial. Only rarely wait for the full suite before creating the PR (e.g. risky changes CI doesn't cover); even then, prefer pushing first and running slow tests in the background so he can review meanwhile.
- Never choose a base from memory. Before creating a branch or PR, or pushing to an existing one, `git fetch` and check live state: whether the default branch has moved and whether the PRs you'd build on are still open. Branch from the latest default branch unless you're stacking on a PR that is still open. If a parent PR has merged, rebase only your new commits onto the default branch (`git rebase --onto origin/main <old-base>`) and retarget the PR. If your own branch's PR has merged, start a fresh branch from the default branch.
- Code comments: only for what the code can't say on its own (a non-obvious why, a constraint, a gotcha). Never narrate the change or echo his instructions back (e.g. a comment explaining that something is "not seeded" right after he said not to seed it). If a change makes a comment unnecessary, delete it instead of rewriting it to describe what's no longer there. Keep comments short and match the file's existing comment density.

Agent-specific instructions live in per-agent directories:

- `claude/` — Claude Code plugin (always-on preferences + model-invoked `skills/`).
- `codex/` — Codex plugin (always-on preferences + task-specific `skills/`).
- `devin/` — Devin plugin (`AGENTS.md` + triggered `rules/`).
