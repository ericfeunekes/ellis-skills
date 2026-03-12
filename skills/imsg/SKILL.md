---
name: imsg
description: iMessage/SMS CLI for reading, searching, and sending messages
---

# Skill: `imsg` CLI

Read, search, and send iMessage/SMS from the terminal.

Run `imsg --help` and `imsg <command> --help` for full command reference.

## Ellis-specific flags (always include)
- `--json` — structured output
- `--limit <n>` — bound result size (for read commands)

## Requirements

- Full Disk Access for terminal (System Settings > Privacy & Security > Full Disk Access)
- Messages.app must be signed in

## List chats

- Recent chats: `imsg chats --limit 10 --json`
- All chats: `imsg chats --json`

## Read chat history

- By chat ID: `imsg history --chat-id <id> --limit 20 --json`
- With attachments: `imsg history --chat-id <id> --limit 20 --attachments --json`
- Date range: `imsg history --chat-id <id> --start 2026-01-01T00:00:00Z --json`

## Send messages

- By phone/email: `imsg send --to +14155551212 --text "message" --json`
- By chat ID: `imsg send --chat-id <id> --text "message" --json`
- With attachment: `imsg send --to +14155551212 --text "message" --file ~/path/to/file --json`
- Service selection: `--service imessage|sms|auto` (defaults to auto)

## Reactions

- React to latest message: `imsg react --chat-id <id> --tapback love --json`

## Search

- Search is done by listing chats and filtering, or by reading history and scanning content.
- Use `--limit` to bound results.

## Output format

- Always use `--json` for structured output.
- Chat objects include: `id`, `identifier` (phone/email), `name`, `service` (iMessage/SMS/RCS), `last_message_at`.
- Message objects include: `id`, `sender`, `text`, `timestamp`, `hasAttachment`.

## Failure handling

- "Bad CPU type": binary needs rebuilding for current architecture.
- Permission denied: suggest granting Full Disk Access to terminal app.
- Messages.app not signed in: report and suggest signing in.

## Safety

- Confirm with the user before sending messages or reactions.
- Handle message content with privacy awareness — don't log or repeat full message bodies unless specifically asked.
