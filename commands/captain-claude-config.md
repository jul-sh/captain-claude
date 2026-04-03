---
description: "View or edit plugin configuration."
---

# /captain-claude:config

View or edit plugin configuration.

## Usage

```
/captain-claude:config                           # view all config
/captain-claude:config <key> <value>             # set a config value
```

## Examples

```
/captain-claude:config claude.model opus
/captain-claude:config max_rounds 15
```

## Behavior

### View (no arguments)

1. Read config via `${CLAUDE_PLUGIN_ROOT}/scripts/config.sh read`.
2. Display all configuration in a readable format, including claude settings, plan settings, max_rounds, and the three instruction sets.
3. Indicate which values come from project-level overrides vs user-level config.

### Set (key + value)

1. Parse the dot-notation key (e.g., `claude.model` → `{"claude": {"model": ...}}`).
2. Write via `${CLAUDE_PLUGIN_ROOT}/scripts/config.sh write <key> <value>`.
3. Confirm the change.

**Supported keys:**
- `claude.model`, `claude.plan_model`, `claude.review_model`
- `plans.directory`, `plans.filename_template`
- `max_rounds`
- For array values (plan_instructions, implementation_instructions, review_instructions), use `/captain-claude:instructions` or edit the config file directly.
