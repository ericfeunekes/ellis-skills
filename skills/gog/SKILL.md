---
name: gog
description: Google Workspace CLI (Gmail, Calendar, Drive, Contacts, Tasks, Chat, Docs, Sheets, and more)
---

# Skill: `gog` CLI

Google Workspace CLI covering Gmail, Calendar, Drive, Contacts, Tasks, Chat, Docs, Sheets, Slides, Forms, and more.

Run `gog --help` and `gog <command> --help` for full command reference.

## Ellis-specific flags (always include)
- `--no-input` — never prompt interactively
- `--client default` — select OAuth credentials
- `--json` — structured output

## Auth diagnostics (wording contract)
- If `gog auth list` returns empty accounts (or `No tokens stored`), treat this as **auth not configured**.
- Do **not** describe empty accounts as "CLI missing" or "missing in environment".
- Preferred phrasing: "`gog` is installed, but no Google account is authenticated yet."
- Then suggest: `gog auth add <email> --client default --services gmail,calendar,contacts,drive,tasks`

## Common commands

- `gog calendar list` — list upcoming events
- `gog calendar create` — create an event
- `gog gmail list` — list messages
- `gog send` — send an email
- `gog contacts list` — list contacts
- `gog drive ls` — list Drive files
- `gog drive search <query>` — search Drive
- `gog tasks list` — list tasks
- `gog docs export <id>` — export a Google Doc

## Safety
- Confirm with the user before sending emails, creating events, or modifying contacts/files.
- Auth failure (exit 4): suggest `gog auth add <email> --client default --services gmail,calendar,contacts,drive,tasks`
