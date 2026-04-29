---
name: setup
description: >-
  First-run setup for the Knowledge Hub. Initializes the Obsidian vault at the configured path
  and verifies the MCP connection. Run this once after installing the plugin.
  Use when: just installed the plugin; vault doesn't exist yet; switching to a new vault location.
---

# Knowledge Hub Setup

Initialize the Knowledge Hub vault and verify the Obsidian MCP connection.

## Workflow

### Step 1 — Determine the vault path

Check if the `KNOWLEDGE_HUB_PATH` environment variable is set by running:
```bash
echo "${KNOWLEDGE_HUB_PATH:-~/.knowledge-hub (default)}"
```

The active vault path is `$KNOWLEDGE_HUB_PATH` if set, otherwise `~/.knowledge-hub`.

Ask the user: **"Where would you like your knowledge vault? Press Enter to use `~/.knowledge-hub`, or provide a custom path."**

If a custom path is chosen, instruct the user to add this to their `~/.zshrc` (or `~/.bash_profile`):
```bash
export KNOWLEDGE_HUB_PATH="/your/custom/path"
```
Then reload: `source ~/.zshrc`

### Step 2 — Check if vault already exists

Run:
```bash
ls "$KNOWLEDGE_HUB_PATH" 2>/dev/null || ls ~/.knowledge-hub 2>/dev/null
```

If `index.md` and `log.md` are already present, the vault is initialized — skip to Step 4.

### Step 3 — Create the vault structure

Create the following directories and files at the vault path:

**Directories:**
```bash
mkdir -p "$VAULT/Projects"
mkdir -p "$VAULT/Tasks"
mkdir -p "$VAULT/Patterns"
mkdir -p "$VAULT/Technologies"
mkdir -p "$VAULT/Lessons"
mkdir -p "$VAULT/Sessions"
```

**`index.md`** — master catalog:
```markdown
# Knowledge Hub Index

Master catalog of all notes. The note-taker updates this file every time it creates or significantly updates a note. The knowledge-retriever reads this file first before drilling into individual notes.

## Format

| Note | Description | Tags | Date |
|---|---|---|---|
| [[filename-without-extension]] | One-line description | #tag1 #tag2 | YYYY-MM-DD |

---

## Projects

| Note | Description | Tags | Date |
|---|---|---|---|

## Tasks

### Bug Fixes

| Note | Description | Tags | Date |
|---|---|---|---|

### Features

| Note | Description | Tags | Date |
|---|---|---|---|

### Refactoring

| Note | Description | Tags | Date |
|---|---|---|---|

### Documentation

| Note | Description | Tags | Date |
|---|---|---|---|

### Configuration

| Note | Description | Tags | Date |
|---|---|---|---|

### Testing

| Note | Description | Tags | Date |
|---|---|---|---|

### Other

| Note | Description | Tags | Date |
|---|---|---|---|

## Patterns

| Note | Description | Tags | Date |
|---|---|---|---|

## Technologies

| Note | Description | Tags | Date |
|---|---|---|---|

## Lessons

| Note | Description | Tags | Date |
|---|---|---|---|

## Sessions

| Note | Description | Tags | Date |
|---|---|---|---|
```

**`log.md`** — activity log:
```markdown
# Activity Log

Append-only chronological record of all knowledge base operations. Each entry is written by the note-taker after completing a write operation.

## Format

New rows are appended to the bottom of the table. Do not modify existing rows.

Valid operations: `note-taken`, `note-updated`, `note-deleted`

---

| Date | Operation | File | Description |
|---|---|---|---|
```

### Step 4 — Verify MCP connection

Use `obsidian/list_directory` to list the vault root. If it returns the folder contents (Projects, Tasks, index.md, etc.), the MCP is connected and working.

If the MCP fails, check:
1. Node.js is installed: `node --version` (required for `npx`)
2. The vault path exists and is readable
3. Restart Claude Code to reload the MCP server

### Step 5 — Confirm setup complete

Report to the user:
- Vault location
- Folders created
- MCP connection status
- Next steps: "You're ready. Claude will now automatically retrieve knowledge at the start of tasks and document learnings at the end."
