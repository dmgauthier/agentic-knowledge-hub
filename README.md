# agentic-knowledge-hub

A Claude Code plugin that gives Claude persistent memory across sessions via an Obsidian vault. Claude automatically retrieves relevant past work at the start of tasks and documents new learnings at the end.

## What it provides

| Component | Description |
|---|---|
| **`knowledge-retriever` agent** | Searches the vault for past solutions, patterns, lessons, and project context |
| **`note-taker` skill** | Documents completed work to the vault — note, index entry, log entry, cross-links |
| **`setup` skill** | First-run vault initialization and MCP verification |
| **Obsidian MCP server** | Gives Claude read/write access to the vault from any working directory |

## Prerequisites

- **Node.js** — required to run the Obsidian MCP server via `npx`

## Installation

```
/plugin install dgauthier/agentic-knowledge-hub
```

Then run setup:

```
run the knowledge-hub setup
```

## Vault location

The vault defaults to `~/.knowledge-hub`. To use a custom path, set `KNOWLEDGE_HUB_PATH` in your shell profile:

```bash
export KNOWLEDGE_HUB_PATH="/path/to/your/vault"
```

You can open the vault folder in [Obsidian](https://obsidian.md) to browse your notes visually.

## Usage

Once installed and set up, the agents work automatically:

- **`knowledge-retriever`** — runs at the start of every substantial task to surface relevant past solutions, patterns, and context
- **`note-taker`** — runs at the end of every substantial task to document what was accomplished

You can also invoke them explicitly:

```
use the knowledge-retriever to find past work on authentication
use the note-taker to document what we just built
```

## Vault structure

```
~/.knowledge-hub/
├── index.md        # Master catalog — read first by the retriever
├── log.md          # Append-only activity log
├── Projects/       # Individual code projects
├── Tasks/          # Completed tasks (bugs, features, refactoring, etc.)
├── Patterns/       # Reusable patterns and best practices
├── Technologies/   # Notes on specific technologies and frameworks
├── Lessons/        # Lessons learned and gotchas
└── Sessions/       # Session-specific artifacts
```

## Verifying the MCP

After installation:

```
use the knowledge-retriever to list what's in my knowledge hub
```

If it returns vault contents, the MCP is connected. If not, confirm `node --version` works and restart Claude Code.
