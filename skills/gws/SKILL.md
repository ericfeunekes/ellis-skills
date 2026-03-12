---
name: gws
description: Google Workspace CLI (Gmail, Calendar, Drive, Contacts, Tasks, Docs, Sheets, and more)
---

# Skill: `gws` CLI

Official Google Workspace CLI covering Gmail, Calendar, Drive, Contacts, Tasks, Docs, Sheets, and more. Dynamically built from Google's Discovery Service — when Google adds API endpoints, `gws` picks them up automatically.

Run `gws --help` and `gws <service> --help` for full command reference.

## Command structure

```
gws <service> <resource> <method> [--params '{}'] [--json '{}']
```

- `--params` — URL/query parameters (calendarId, maxResults, etc.)
- `--json` — request body for create/update operations

## Auth diagnostics

- Run `gws auth status` to check authentication state
- If `auth_method` is `none`, auth is not configured
- Preferred phrasing: "`gws` is installed, but no Google account is authenticated yet."
- Then suggest: `gws auth login -s calendar,gmail,drive,contacts,tasks,docs,sheets`

## Common commands

### Calendar
```bash
# List upcoming events
gws calendar events list --params '{"calendarId":"primary","maxResults":10,"timeMin":"2026-03-07T00:00:00Z","singleEvents":true,"orderBy":"startTime"}'

# Create an event
gws calendar events insert --params '{"calendarId":"primary"}' --json '{"summary":"Meeting","start":{"dateTime":"2026-03-08T14:00:00-04:00"},"end":{"dateTime":"2026-03-08T15:00:00-04:00"}}'

# Delete an event
gws calendar events delete --params '{"calendarId":"primary","eventId":"<id>"}'
```

### Gmail
```bash
# List messages
gws gmail users messages list --params '{"userId":"me","maxResults":10}'

# Send a message (draft first, then send)
gws gmail users messages send --params '{"userId":"me"}' --json '{"raw":"<base64-encoded-email>"}'
```

### Drive
```bash
# List files
gws drive files list --params '{"pageSize":10}'

# Search files
gws drive files list --params '{"q":"name contains \"report\"","pageSize":10}'
```

### Contacts
```bash
# List contacts
gws people people connections list --params '{"resourceName":"people/me","personFields":"names,emailAddresses"}'
```

### Tasks
```bash
# List task lists
gws tasks tasklists list

# List tasks in a list
gws tasks tasks list --params '{"tasklist":"<tasklistId>"}'
```

## Calendar Workflows

### Creating events
**Always check availability before creating.** Before inserting a calendar event:
1. List events for the target time window to check for conflicts
2. If conflicts exist, surface them and ask the user how to proceed
3. If clear, confirm the event details and create it

### When the user explicitly requests an action
If the user says "block 45 minutes at 7pm" or "schedule time for X", this is an instruction to act — not a request for discussion. Follow the workflow above, then create the event. Don't ask "would you like me to create this?" when they've already asked you to.

### Rescheduling
When the user asks to move/reschedule tasks:
1. Check availability at the new time
2. Create the new time block
3. Confirm what was done

## Safety
- For ambiguous requests, confirm intent before creating events
- For explicit instructions ("block time", "schedule", "create an event"), proceed after conflict check
- Never send emails without explicit user approval
- Auth failure: suggest `gws auth login -s calendar,gmail,drive,contacts,tasks,docs,sheets`

## Migration note
This replaces the previous `gog` CLI. Command structure differs — `gws` uses Google API resource/method naming directly.
