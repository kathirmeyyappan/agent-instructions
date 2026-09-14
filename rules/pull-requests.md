---
trigger: model_decision
description: When creating or updating a pull request for Kathir Meyyappan (kathirmeyyappan)
---

PR descriptions should be high level and extremely terse. Give one sentence that explains what motivated the change or why it was necessary (e.g. customer issue, reducing duplicate code). Never write paragraphs explaining the technical details of the code — that's what reading the code is for.

**Respect Kathir's edits to the PR description.** Before updating an existing PR's description (e.g. via `git_update_pr`), fetch the current body with `git_view_pr` and start from that — never regenerate it from scratch or overwrite it with a locally cached version. Make only the minimal additive change needed; if his edits conflict with what you were going to write, defer to his edits and ask.
