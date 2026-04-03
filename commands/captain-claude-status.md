---
description: "Show current phase, round, and review history."
---

# /captain-claude:status

Shows current pipeline state for an active captain-claude run.

## Behavior

1. Check if `.captain-claude/state.json` exists in the current project directory.

2. If no state file exists:
   ```
   No active captain-claude run in this project.
   ```

3. If state file exists, read it and display:

   ```
   ## captain-claude Status

   **Phase:** <phase>
   **Task:** <task_description>
   **Plan:** <plan_file>
   **Round:** <round> / <max_rounds>
   **Started:** <started_at>

   ### Review History
   | Round | Verdict | Summary | Time |
   |-------|---------|---------|------|
   | 1     | REJECT  | ...     | ...  |
   | 2     | REJECT  | ...     | ...  |
   ```

4. If phase is "review" or "implementing", show session info from state if available.

5. If phase is "complete":
   ```
   **Status:** Complete — reviewer approved on round <N>
   ```

6. If phase is "failed":
   ```
   **Status:** Failed — <reason from state>
   ```
