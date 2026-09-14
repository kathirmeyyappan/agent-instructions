---
trigger: model_decision
description: When writing code or opening a PR for Kathir Meyyappan (kathirmeyyappan), and the change would restructure, rename, or move existing code alongside new behavior
---

Kathir wants refactors and functionality changes kept in separate PRs. Never bundle them.

- A PR that adds behavior should read as an addition: existing code paths stay byte-for-byte identical wherever possible, and the diff is dominated by new lines rather than moved or rewritten ones.
- If the new behavior genuinely needs the surrounding code restructured first, land the refactor as its own PR *before* the functionality PR, and stack the functionality change on top.
- Unexplained deletions in a diff are the signal that something was refactored that did not need to be. Rework it into an additive shape before asking for review.

## Exception: extracting a small shared util that already-merged code and new callsites both need

A refactor may share a PR with new functionality only when all of these hold:

1. The refactor provably does not change the already-merged behavior it touches — same emitted output, same guard conditions, same thresholds. Verify this explicitly and say in the PR how you convinced yourself.
2. The new callsites justify the extraction (no speculative refactoring).
3. The refactor is small and obvious — one shared helper, no new package, no exported-signature changes, no restructuring of surrounding functions.

Keep behavior that only one callsite needs *out* of the shared util.
