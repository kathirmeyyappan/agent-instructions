# agent-instructions

Kathir Meyyappan's personal instructions for coding agents. Intentionally small; grows as needed.

- `AGENTS.md` — high-level, agent-agnostic rules.
- `claude/` — Claude Code plugin: always-on preferences plus model-invoked skills in `claude/skills/`.
- `devin/` — Devin plugin: always-on `AGENTS.md` plus triggered rules in `devin/rules/`.

## Install

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
