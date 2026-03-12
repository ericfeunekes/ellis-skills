---
name: agent-browser
description: Headless browser automation for AI agents
---

# Skill: `agent-browser` CLI

Headless browser automation for AI agents. Navigate, interact, screenshot, and extract content from web pages.

Run `agent-browser --help` for full command reference.

## Key patterns
- `agent-browser snapshot -i` — get interactive elements with refs (start here)
- `agent-browser click @e2` — interact by ref from snapshot
- `agent-browser get text @e1` — extract text content
- `agent-browser screenshot` — capture current state

## Ellis-specific flags (always include)
- `--json` — structured output where available

## Safety
- Do not submit forms, make purchases, or authenticate to services without user confirmation.
- Do not store or log credentials entered via `fill` or `set credentials`.
- Use `--session <name>` to isolate browser state between tasks.
