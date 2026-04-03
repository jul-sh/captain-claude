# captain-claude

Claude Code plugin that runs a structured plan/implement/review loop — all powered by Claude. Claude plans, Claude implements, Claude reviews; loop until the reviewer is satisfied.

## What It Does

One command. You describe what you want; include ad-hoc instructions for any phase in natural language.

```
/captain-claude refactor mac app to enable ios app with code sharing
```

```
/captain-claude refactor auth module. for planning, focus on backwards compat. when implementing, don't touch the database layer. reviewer should be strict about test coverage.
```

Ad-hoc instructions are merged with your configured defaults for each phase.

## Why

Claude is a strong implementor; fast, creative, good across large codebases. But it reward-hacks. It takes shortcuts to look done: skips edge cases, writes tests that pass without verifying behavior, deviates from plans when compliance is hard, declares victory early. You need a separate verifier.

A structured plan/implement/review loop with separate sessions catches issues that a single pass misses. The planner reasons about architecture upfront. The implementor executes. The reviewer evaluates against the plan with fresh eyes — a separate session, a separate prompt, no anchoring to the implementation's internal narrative. Even with the same model, the separation of concerns and session isolation produces better results than a single-shot approach.

For cross-model review (using a different model for planning/review), see [captain-codex](https://github.com/jul-sh/captain-codex).

## Dependencies

- Claude Code v2.1.34+
- `jq`

## Installation

This plugin is available through the [jul-sh Claude Code plugin marketplace](https://github.com/jul-sh/claude-plugins).

### Add the marketplace:
```
/plugin marketplace add jul-sh/claude-plugins
```

### Install the plugin:
```
/plugin install captain-claude@jul-sh
```

## Commands

| Command | Description |
|---------|-------------|
| `/captain-claude <task>` | Full pipeline: plan, implement, review loop |
| `/captain-claude:status` | Current phase, round, review history |
| `/captain-claude:instructions` | View/edit plan, implementation, and review instructions |
| `/captain-claude:config` | View/edit plugin config |

Flags: `--skip-plan <path>`, `--max-rounds <n>`, `--supervised`.

## How It Works

**Planning.** Claude reads the codebase and writes an implementation plan. Saved to `tasks/<slug>.md`. Reviews happen in the same Claude session, so the reviewer retains full context of the plan.

**Implementation.** Claude receives the plan and implements autonomously, maintaining a worklog in the plan file.

**Review loop.** When Claude finishes, a Stop hook dispatches a separate Claude session for review. Rejected; Claude gets feedback and continues. Approved; done. Max rounds exceeded; you decide.

**Supervised mode.** `--supervised` pauses after planning and after each review round for human approval.

## Configuration

Three instruction sets control what each phase does:

| Config key | Controls |
|---|---|
| `plan_instructions` | What Claude should focus on when planning |
| `implementation_instructions` | How Claude should implement |
| `review_instructions` | What Claude should check during review |

Edit via `/captain-claude:instructions` or directly in config files.

User-level: `~/.captain-claude/config.json`
Project-level override: `.captain-claude/config.json`

```
/captain-claude:config                           # view all
/captain-claude:config claude.model opus         # set a value
/captain-claude:config max_rounds 15             # set a value
```

See `templates/default-config.json` for all options.

## License

MIT
