# Kathir's workflow

These rules apply to every session Kathir Meyyappan (git username `kathirmeyyappan`, email `kathir@modal.com` / `kathirmey@gmail.com`) requests.

- Branch names: always `kathir/` plus a short description of the change (e.g. `kathir/fix-auth-token-cache`). Both halves are required. Never accept a default or auto-generated branch name such as `claude/busy-volta-q4e53t` or `devin/1234-...`; rename it before the first push.
- PR titles: short and scoped to what the change does (e.g. `Add Modal skill`, `Fix token cache expiry`). Never long-winded, and never reference the conversation ("as discussed", "per Kathir's request", "follow-up fix").
- Pushing and opening PRs: judge whether local checks are worth the wait. For straightforward or trivial changes, push and open the PR immediately; CI runs anyway. Don't manually run the full pre-commit/test suite first, and `--no-verify` is fine when installed hooks are slow and the change is trivial. Only rarely wait for the full suite before creating the PR (e.g. risky changes CI doesn't cover); even then, prefer pushing first and running slow tests in the background so he can review meanwhile.
- Code comments: only for what the code can't say on its own (a non-obvious why, a constraint, a gotcha). Never narrate the change or echo his instructions back (e.g. a comment explaining that something is "not seeded" right after he said not to seed it). If a change makes a comment unnecessary, delete it instead of rewriting it to describe what's no longer there. Keep comments short and match the file's existing comment density.
- See the triggered rules in `rules/` for PR descriptions, the test-plan check, post-review pushes and Modal development.
