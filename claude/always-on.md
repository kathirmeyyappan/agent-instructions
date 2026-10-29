# Kathir's workflow

These preferences apply to every session for Kathir Meyyappan (git username `kathirmeyyappan`, email `kathir@modal.com` / `kathirmey@gmail.com`).

- Default to the shortest answer that fully answers the question. Lead with the answer, not the reasoning that got there.
- No preamble, no recap of the question, no restating what you're about to do, no closing summary.
- Don't volunteer adjacent findings, caveats, or design opinions unless they change the answer. Offer them in one line at most ("worth a look if you want: X") rather than explaining them in depth.
- When he asks for details or exact mechanics, be precise and complete but tight: name the file/function, quote the relevant lines, state the conclusion. Long is only acceptable when the extra length is all substance.
- If he says "briefly", "quick question", or "don't rabbithole", answer in 1-3 sentences and stop.
- Branch names: always `kathir/` plus a short description of the change (e.g. `kathir/fix-auth-token-cache`). Both halves are required. Never accept a default or auto-generated branch name such as `claude/busy-volta-q4e53t` or `devin/1234-...`; rename it before the first push.
- PR titles: short and scoped to what the change does (e.g. `Add Modal skill`, `Fix token cache expiry`). Never long-winded, and never reference the conversation ("as discussed", "per Kathir's request", "follow-up fix").
- Pushing and opening PRs: judge whether local checks are worth the wait. For straightforward or trivial changes, push and open the PR immediately; CI runs anyway. Don't manually run the full pre-commit/test suite first, and `--no-verify` is fine when installed hooks are slow and the change is trivial. Only rarely wait for the full suite before creating the PR (e.g. risky changes CI doesn't cover); even then, prefer pushing first and running slow tests in the background so he can review meanwhile.
- Never choose a base from memory. Before creating a branch or PR, or pushing to an existing one, `git fetch` and check live state: whether the default branch has moved and whether the PRs you'd build on are still open. Branch from the latest default branch unless you're stacking on a PR that is still open. If a parent PR has merged, rebase only your new commits onto the default branch (`git rebase --onto origin/main <old-base>`) and retarget the PR. If your own branch's PR has merged, start a fresh branch from the default branch.
