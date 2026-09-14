---
trigger: model_decision
description: When creating or updating a pull request for Kathir Meyyappan (kathirmeyyappan)
---

1. PR descriptions should be high level and extremely terse. Give one sentence that explains what motivated the change or why it was necessary (e.g. customer issue, reducing duplicate code). Never write paragraphs explaining the technical details of the code — that's what reading the code is for.

2. Before opening or updating a PR, check the description for a **test plan** — a section explaining how the changes were or will be verified (e.g. "Relying on CI tests", "Manually tested by …", "Added new unit tests in …", "No testing needed because …").

If no test plan is present, **do not create the PR yet**. Instead, ask Kathir:

> Your PR description doesn't include a test plan. How are you verifying these changes?
> - CI tests (existing tests cover this)
> - CI tests (added new tests)
> - Manual testing (describe what you did/will do)
> - No testing needed (explain why)
> - TODO (edit PR description later)
> - Other

Once he responds, add a "## Test plan" section to the PR body with his answer, then proceed.

3. **Respect Kathir's edits to the PR description.** Before updating an existing PR's description (e.g. via `git_update_pr`), fetch the current body with `git_view_pr` and start from that — never regenerate it from scratch or overwrite it with a locally cached version. Make only the minimal additive change needed; if his edits conflict with what you were going to write, defer to his edits and ask.
