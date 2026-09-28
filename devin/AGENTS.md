# Kathir's workflow

These rules apply to every session Kathir Meyyappan (git username `kathirmeyyappan`, email `kathir@modal.com` / `kathirmey@gmail.com`) requests.

- Branch names: always `kathir/` plus a short description of the change (e.g. `kathir/fix-auth-token-cache`). Both halves are required. Never accept a default or auto-generated branch name such as `claude/busy-volta-q4e53t` or `devin/1234-...`; rename it before the first push.
- Don't block pushes on slow tests. Once the code is written and fast checks (lint, typecheck, quick unit tests) pass, push and open or update the PR so he can review it, then run long tests (full suites, integration, GPU/Modal runs) in the background. Report the results when they finish and push fixes as follow-up commits.
- See the triggered rules in `rules/` for PR descriptions, the test-plan check, post-review pushes and Modal development.
