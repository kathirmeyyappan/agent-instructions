# agent-instructions

Kathir Meyyappan's personal instructions for coding agents. Intentionally small; grows as needed.

- `AGENTS.md` — high-level, agent-agnostic rules.
- `claude/` — Claude Code plugin: always-on preferences plus model-invoked skills in `claude/skills/`.
- `codex/` — Codex plugin: always-on preferences plus task-specific skills.
- `devin/` — Devin plugin: always-on `AGENTS.md` plus triggered rules in `devin/rules/`.

## Install

**Codex** (one-time setup; no checkout to manage):

```sh
codex plugin marketplace add kathirmeyyappan/agent-instructions
codex plugin add kathir-workflow@agent-instructions
```

Restart Codex, then review and trust the plugin's startup hooks when prompted. Until trusted, Codex skips the always-on instructions. The plugin applies across projects and includes Modal, pull request, and post-review skills. Codex manages its cached files; no working checkout is required. Lifecycle hooks require a supported local Codex runtime and do not run in cloud-orchestrated ChatGPT Work.

To refresh from GitHub later: `codex plugin marketplace upgrade agent-instructions`.

**Devin**: Settings → Personal → Plugins, add `kathirmeyyappan/agent-instructions#devin` as a required plugin.

**Claude Code**:

```
/plugin marketplace add kathirmeyyappan/agent-instructions
/plugin install kathir-workflow@agent-instructions
```

The plugin is user-level, so it applies to every project. It carries:

- `claude/always-on.md` — brevity and branch-naming preferences, injected each session by a `SessionStart` hook (a plugin has no always-on instructions file; the hook is the equivalent).
- `claude/skills/` — one skill per rule, invoked by Claude when the skill's `description` matches the task. This is the analogue of Devin's `trigger: model_decision`.

Both plugins include a `modal` skill/rule that makes the agent a Modal SDK expert, grounded in the `modal-labs/modal` monorepo source, the changelog, and modal.com docs.

Alternative to the hook, if you'd rather keep always-on rules in memory: import this repo's root `AGENTS.md` from `~/.claude/CLAUDE.md` with `@~/path/to/agent-instructions/AGENTS.md`.
