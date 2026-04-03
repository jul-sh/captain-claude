# captain-claude

Claude Code plugin: Claude plans, Claude implements, Claude reviews.

## Layout

- `commands/` — skill markdown files (the command definitions)
- `hooks/` — shell scripts (Stop event hook)
- `scripts/` — shell utilities (config, planning, review prompt)
- `templates/` — prompt skeletons and default config

## Config resolution

`templates/default-config.json` ← `~/.captain-claude/config.json` ← `.captain-claude/config.json`
