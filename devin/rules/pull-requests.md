---
trigger: model_decision
description: When creating or updating a pull request for Kathir Meyyappan (kathirmeyyappan)
---

PR descriptions should be high level and extremely terse. Give one sentence that explains what motivated the change or why it was necessary (e.g. customer issue, reducing duplicate code). Never write paragraphs explaining the technical details of the code — that's what reading the code is for.

**Respect Kathir's edits to the PR description.** Before updating an existing PR's description (e.g. via `git_update_pr`), fetch the current body with `git_view_pr` and start from that — never regenerate it from scratch or overwrite it with a locally cached version. Make only the minimal additive change needed; if his edits conflict with what you were going to write, defer to his edits and ask.

**Titles** read like a terse commit subject: an imperative verb, roughly 3-7 words, no articles ("the", "a") or possessives, and no list of every fix. Name the overall scope, not each item: `Fix Colgera level bugs`, not `Colgera fixes: the theme's level, the hint's name, the ice, the pool`. Other good examples: `Add Modal skill`, `Fix token cache expiry`. Never reference the conversation that produced the change ("as discussed", "per Kathir's request", "follow-up fix"). The PR description carries the details.
