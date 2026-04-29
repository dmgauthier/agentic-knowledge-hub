---
name: note-taker
description: >-
  Documents completed work, patterns, and learnings to the Obsidian Knowledge Hub.
  INVOKE THIS SKILL AS THE FINAL STEP after completing any substantial task.
  Use when: a bug fix, feature, or refactoring is done; a new reusable pattern or technique is discovered;
  important lessons or gotchas are learned; a new project or technology is encountered for the first time.
---

# Note Taker

Document completed work directly into the Obsidian Knowledge Hub using the obsidian MCP tools. You have full conversation context — use it to write rich, accurate notes that capture the *why*, not just the *what*.

The vault is located at `~/.knowledge-hub` by default (or `$KNOWLEDGE_HUB_PATH` if set).

## When to Invoke

- A significant task is complete (bug fix, feature, refactoring, configuration, etc.)
- A new reusable pattern or technique was discovered
- An important lesson or gotcha was learned
- A new project or technology was encountered for the first time

---

## Workflow

### Step 1 — Check for Duplicates

Read `index.md` at the vault root with `obsidian/read_note`. Scan for existing notes on the same topic. If a similar note exists, **update it** rather than creating a new one.

### Step 2 — Determine Category and Filename

| What happened | Folder | Template |
|---|---|---|
| Bug fix, feature, refactor, docs, config, testing | `Tasks/` | Task template |
| Reusable technique or approach | `Patterns/` | Pattern template |
| New project encountered | `Projects/` | Project template |
| Framework, library, or tool notes | `Technologies/` | Technology template |
| Mistake, gotcha, or insight | `Lessons/` | Lesson template |

Filename: kebab-case, descriptive (e.g. `fix-auth-token-refresh.md`, `llm-wiki-pattern.md`).

### Step 3 — Write the Note

Use `obsidian/write_note` with `mode: "overwrite"` and the appropriate template below. Fill every section — don't leave placeholders.

### Step 4 — Update `index.md` (required)

1. Read the current `index.md` with `obsidian/read_note`
2. Find the correct section table and add a new row:
   ```
   | [[filename-without-extension]] | One-line description | #tag1 #tag2 | YYYY-MM-DD |
   ```
3. Rewrite with `obsidian/write_note` (`mode: "overwrite"`)

The sections in `index.md` map to vault folders: Projects, Tasks (with subsections by type), Patterns, Technologies, Lessons, Sessions. Place the row under the most specific matching subsection.

### Step 5 — Append to `log.md` (required)

Use `obsidian/write_note` with `mode: "append"` to add a new table row:
```
| YYYY-MM-DD | note-taken | Category/filename.md | One-line summary of what was documented |
```

Use `note-updated` instead of `note-taken` when updating an existing note.

### Step 6 — Cross-link Related Notes (required when strong connections exist)

Find 2–3 directly related existing notes using `obsidian/search_notes`. For each, add a `[[WikiLink]]` to the new note using `obsidian/patch_note`. Strong connections are:
- The **project note** when creating a task note (add new task to project's Related Tasks list)
- A **pattern note** when the task used or reinforced that pattern
- A **technology note** when the task introduced a meaningful gotcha or usage example

Skip this step for narrow, standalone notes with no clear strong connection.

---

## Note Templates

### Task
```markdown
---
type: task
category: [bug-fix|feature|refactoring|documentation|testing|configuration]
project: "[[Project Name]]"
date: YYYY-MM-DD
tags: [relevant, tags]
---

# [Task Title]

## Problem
Clear description of what needed to be solved.

## Approach
Steps taken and key decisions made.

## Solution
Key implementation details. Focus on the *why* behind choices.

## Related
- Patterns: [[Pattern Name]]
- Technologies: [[Tech Name]]
- Lessons: [[Lesson Name]]
```

### Pattern
```markdown
---
type: pattern
category: [design|code|testing|architecture|debugging]
tags: [relevant, tags]
---

# [Pattern Name]

## Use Cases
When to apply this pattern.

## Description
What the pattern is and what problem it solves.

## Implementation
How to implement, with concrete examples.

## Pros & Cons
Trade-offs and considerations.

## Examples
- [[Project Name]] — context and outcome
```

### Project
```markdown
---
type: project
tech-stack: [languages, frameworks, tools]
date: YYYY-MM-DD
tags: [relevant, tags]
---

# [Project Name]

## Overview
Brief description of the project and its purpose.

## Tech Stack
- Language(s):
- Framework(s):
- Key Libraries:
- Tools:

## Architecture
High-level structure and key components.

## Key Files/Directories
Important locations in the codebase.

## Related Notes
- Tasks: [[Task Name]]
- Patterns: [[Pattern Name]]
- Technologies: [[Tech Name]]
```

### Technology
```markdown
---
type: technology
category: [language|framework|library|tool|api]
tags: [relevant, tags]
---

# [Technology Name]

## Overview
What it is and what it's used for.

## Key Concepts
Important concepts and terminology.

## Common Tasks
Frequent operations and how to perform them.

## Gotchas & Tips
Things to watch out for and useful tips.

## Usage
- [[Project Name]] — how it's used
```

### Lesson
```markdown
---
type: lesson
category: [mistake|performance|security|best-practice|troubleshooting]
date: YYYY-MM-DD
tags: [relevant, tags]
---

# [Lesson Title]

## Context
Where this lesson came from.

## What Happened
Description of the situation.

## The Lesson
What was learned — the *why* matters most here.

## Why It Matters
Impact and importance.

## How to Apply
Practical steps for the future.
```

---

## Guidelines

- **Use consistent naming**: kebab-case filenames (e.g. `fix-authentication-bug.md`)
- **Focus on "why"**: Capture reasoning and tradeoffs, not just outcomes
- **`index.md` is authoritative**: The per-folder index files are legacy — always update root `index.md`
- **Be concise but complete**: Enough detail to be useful months later without being verbose
- **Date everything**: Use today's date in frontmatter
- **All steps are required**: write note → update index.md → append log.md → cross-link (when applicable)
