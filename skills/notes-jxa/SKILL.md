---
name: notes-jxa
description: Apple Notes management via JXA (JavaScript for Automation)
---

# Skill: Apple Notes via JXA

Manage Apple Notes using JavaScript for Automation (`osascript -l JavaScript`). Full CRUD with native JSON output.

## Requirements

- Automation permission for terminal to control Notes.app (granted on first use)

## List notes

List notes in a folder with metadata:
```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const folder = Notes.folders.byName("Notes");
const notes = folder.notes();
JSON.stringify(notes.slice(0, 20).map(n => ({
  name: n.name(),
  id: n.id(),
  modDate: n.modificationDate().toISOString(),
})));
'
```

## List folders

```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
JSON.stringify(Notes.folders().map(f => f.name()));
'
```

## Search notes by name

```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const results = Notes.notes.whose({name: {_contains: "search term"}})();
JSON.stringify(results.map(n => ({
  name: n.name(),
  id: n.id(),
  folder: n.container().name(),
  modDate: n.modificationDate().toISOString(),
})));
'
```

## Read note body

```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const results = Notes.notes.whose({name: {_contains: "note title"}})();
const n = results[0];
JSON.stringify({
  name: n.name(),
  id: n.id(),
  body: n.plaintext(),
});
'
```

## Create note

```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const folder = Notes.folders.byName("Notes");
const note = Notes.Note({name: "Note Title", body: "<div>Note content here.</div>"});
folder.notes.push(note);
JSON.stringify({name: note.name(), id: note.id()});
'
```

Note body uses HTML. Use `<div>` for paragraphs, `<br>` for line breaks.

## Update note

Append to existing note:
```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const results = Notes.notes.whose({name: {_contains: "note title"}})();
const n = results[0];
n.body = n.body() + "<div>Appended content.</div>";
JSON.stringify({name: n.name(), id: n.id()});
'
```

Replace note body:
```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const results = Notes.notes.whose({name: {_contains: "note title"}})();
const n = results[0];
n.body = "<div>Completely new content.</div>";
JSON.stringify({name: n.name(), id: n.id()});
'
```

## Delete note

```bash
osascript -l JavaScript -e '
const Notes = Application("Notes");
const results = Notes.notes.whose({name: {_contains: "note title"}})();
Notes.delete(results[0]);
JSON.stringify({deleted: true});
'
```

## Failure handling

- If Notes.app is not running, osascript will launch it automatically.
- If folder not found, report the error and list available folders.
- If no notes match the search, return an empty array.
- Permission dialog on first run: user must click "OK" to grant Automation access.

## Safety

- Confirm with the user before creating, updating, or deleting notes.
- When updating, show the note name and preview of changes before applying.
- Search by name is case-insensitive (`_contains`).
