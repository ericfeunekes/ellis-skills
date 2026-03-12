---
name: remindctl
description: Apple Reminders CLI with full CRUD
---

# Skill: `remindctl` CLI

Manage Apple Reminders from the terminal. Full CRUD with `--json` output.

Run `remindctl --help` and `remindctl <command> --help` for full command reference.

## Ellis-specific flags (always include)
- `--json` — structured output

## View reminders

- Today: `remindctl show today --json`
- Tomorrow: `remindctl show tomorrow --json`
- This week: `remindctl show week --json`
- Overdue: `remindctl show overdue --json`
- Upcoming: `remindctl show upcoming --json`
- Completed: `remindctl show completed --json`
- All: `remindctl show all --json`
- Specific date: `remindctl show 2026-02-21 --json`

## Manage lists

- List all lists: `remindctl list --json`
- Show a list: `remindctl list "Personal" --json`
- Create list: `remindctl list "Projects" --create --json`
- Rename list: `remindctl list "Work" --rename "Office" --json`
- Delete list: `remindctl list "Work" --delete --json`

## Create reminders

- Quick add: `remindctl add "Buy milk" --json`
- With list: `remindctl add --title "Call mom" --list "Personal" --json`
- With due date: `remindctl add --title "Call mom" --list "Personal" --due tomorrow --json`
- Full: `remindctl add --title "Call mom" --list "Personal" --due 2026-02-22 --notes "Check on her health" --json`

## Edit reminders

- Edit by ID: `remindctl edit <id> --title "New title" --json`
- Change due date: `remindctl edit <id> --due 2026-03-01 --json`

## Complete / delete

- Complete by ID: `remindctl complete <id> --json`
- Complete multiple: `remindctl complete <id1> <id2> <id3> --json`
- Delete: `remindctl delete <id> --force --json`

## Date formats

- Natural: `today`, `tomorrow`, `yesterday`
- ISO: `2026-02-21`, `2026-02-21 14:30`
- Always provide timezone-aware dates when possible.

## Failure handling

- If remindctl hangs: macOS Reminders permission not granted. Suggest: System Settings > Privacy & Security > Reminders.
- If reminder ID not found: report the invalid ID and suggest listing first.
- Always include `--json` for parseable error output.

## Safety

- Confirm with the user before completing or deleting reminders.
- When creating reminders, echo back the details before confirming.
