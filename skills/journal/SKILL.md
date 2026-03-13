---
name: journal
description: "Write, search, and review project development journal entries. Use when: logging what was done in a session, searching past decisions, or retrieving context from prior work. PROACTIVE: When starting work on a topic or before making architectural decisions, invoke `/journal check` to surface relevant past decisions and context."
argument-hint: "<command> [args] — write | search | last | check | init | help"
---

# Development Journal Manager

Manage a per-project development journal with sequenced, dated entries.

## Commands

Parse `$ARGUMENTS` to determine the command:

| Invocation | Action |
|------------|--------|
| `/journal` (no args) | Write a new journal entry (default action) |
| `/journal <title>` | Write a new entry with the given title |
| `/journal search <query>` | Search journal by filename, then full text |
| `/journal last [N]` | Show last N entries (default 1) for context |
| `/journal check` | Find journal entries relevant to the current conversation |
| `/journal init` | Create the journal directory, README, GUIDE.md, CLAUDE.md @import, and first entry |
| `/journal help` | Show available commands and usage |

## Journal Location

The journal directory is always `docs/journal/`. Create it if it doesn't exist.

## File Naming Convention

```
YYYY-MM-DD--NNN-short-description.md
```

- `YYYY-MM-DD` — today's date
- `NNN` — three-digit sequence number, zero-padded (001, 002, ...), **per-day**
- `short-description` — lowercase, hyphen-separated, derived from the title

The sequence number resets for each day. To determine the next number: find all existing entries for today's date (`YYYY-MM-DD--*`), take the highest `NNN` among them, and add 1. If no entries exist for today, start at 001.

## Commands in Detail

### Default: Write a new entry

The default action (no subcommand, or any text that isn't `search`, `last`, `check`, `help`, or `init`) is to **write a new journal entry**.

**Title generation:** If the user provides a title after `/journal`, use it. If `/journal` is invoked with no arguments, generate a title yourself by reviewing what was accomplished in the current conversation — look at files modified, decisions made, and topics discussed. The title should be concise (3-8 words) and describe the main outcome, not the process.

**Steps:**

1. If `docs/journal/` doesn't exist or has no `README.md`, run the full `init` command first (see `init` below), then continue.
2. Scan all existing filenames to find the highest sequence number.
3. Slugify the title: lowercase, replace spaces/special chars with hyphens, collapse multiple hyphens, trim to ~60 chars at a word boundary.
4. Review the current conversation to understand what was done — look at tool calls, file edits, decisions, and discussion topics.
5. Create the file with this template:

```markdown
# NNN — <Title>

**Date:** YYYY-MM-DD
**Scope:** <infer from the work done>

## Summary

<One-paragraph summary of what was done and why — written by reviewing the conversation.>

## What changed

<Detailed description. Use tables for file lists, decisions, comparisons. Use bullet lists for sequential steps or findings.>

## Context

<Why this matters. Link to related entries or docs if relevant.>
```

6. Respond with: the filename, sequence number, and a one-line confirmation.

### `search <query>`

Two-phase search:

**Phase 1 — Filename search:**
Use Glob to find journal entries whose filenames contain the query terms (case-insensitive). If matches are found, list them with their title (extracted from the `# NNN —` heading) and date.

**Phase 2 — Full-text search (automatic if Phase 1 finds fewer than 3 results):**
Use Grep to search the content of all journal entries for the query. Show matching entries with the relevant excerpt (a few lines of context around each match).

Present results as a table:

```
| # | Date | Entry | Match |
|---|------|-------|-------|
| 019 | 2026-03-10 | DESPEE naming across docs | filename |
| 011 | 2026-03-05 | Monolithic DSP + ESP32 display | content: "DESPEE" on line 42 |
```

### `last [N]`

1. List all journal entries sorted by filename (which gives chronological + sequence order).
2. Read the last N entries (default 1).
3. For each entry, output: sequence number, date, title, and a 1-2 sentence summary (from the Summary section or first paragraph).
4. If N is 1, also print the full entry content.

### `help`

Print the following command reference to the user:

```
## Journal Commands

| Command | Description |
|---------|-------------|
| `/journal` | Write a new entry (auto-generates title from conversation) |
| `/journal <title>` | Write a new entry with the given title |
| `/journal search <query>` | Search entries by filename and content |
| `/journal last [N]` | Show last N entries (default 1) |
| `/journal check` | Find entries relevant to the current conversation |
| `/journal init` | Set up the journal directory, README, GUIDE.md, and CLAUDE.md @import |
| `/journal help` | Show this help message |

Entries are stored in `docs/journal/` as `YYYY-MM-DD--NNN-description.md`.
```

No other action is needed — just display the help text and stop.

### `check`

Proactively find journal entries relevant to the current conversation. Designed to be invoked by Claude automatically when starting work or before architectural decisions — not only on explicit user command.

**Steps:**

1. Check if `docs/journal/` exists. If not, inform the user there is no journal yet and suggest `/journal init`.
2. **Gather context clues** from the current conversation:
   - File paths being edited or discussed (extract base names and directory names)
   - Key concepts, component names, or technical terms mentioned
   - Any error messages or issue descriptions
3. **Search by filename** (Glob): For each context clue, search for journal entries whose filenames contain the term (e.g., `*auth*`, `*migration*`).
4. **Search by content** (Grep): For each context clue, search journal entry contents. Limit to the top 5 most relevant matches.
5. **Read and summarize**: Read each matched entry's Summary and Context sections. Deduplicate entries found by multiple clues.
6. **Present results** as a brief summary:

```
## Relevant journal entries

| # | Date | Entry | Relevance |
|---|------|-------|-----------|
| 014 | 2026-03-08 | Auth middleware rewrite | mentions `auth.ts`, decided on JWT over sessions |
| 009 | 2026-03-03 | Database migration strategy | discusses migration patterns relevant to current work |

### Key decisions from past entries
- **Entry 014:** Chose JWT tokens over server-side sessions because of horizontal scaling requirements.
- **Entry 009:** All migrations must be reversible; use a down() function in every migration file.
```

7. If no relevant entries are found, say so briefly and continue with the user's task.

### `init`

1. Create the `docs/journal/` directory if it doesn't exist.
2. Write a `README.md` inside the journal directory:

```markdown
# <Project Name> Development Journal

Chronological log of design decisions, implementation sessions, and architecture changes.

Entries are named `YYYY-MM-DD--NNN-description.md` where `NNN` is a global sequence number.

To add an entry: `/journal <title>`
To search: `/journal search <query>`
To review recent context: `/journal last [N]`
For help: `/journal help`
```

For `<Project Name>`, read the repo's top-level `README.md` heading or use the directory name.

3. **Write `docs/journal/GUIDE.md`** with the following content:

   ```markdown
   ## Journal

   This project maintains a development journal in `docs/journal/`. Use `/journal` to log decisions.

   - **Before starting work:** Run `/journal check` to surface past decisions relevant to the files or topics you're about to touch.
   - **Before architectural decisions:** Search the journal (`/journal search <topic>`) to see if this was discussed before.
   - **After completing work:** Write a journal entry (`/journal`) capturing what was done and why.
   ```

4. **Update or create `CLAUDE.md`** in the project root:
   - If `CLAUDE.md` already exists: append the line `@docs/journal/GUIDE.md` to the end of the file (on its own line).
   - If `CLAUDE.md` does not exist: create it with just the line `@docs/journal/GUIDE.md`.

5. **Create the first journal entry** using the standard write flow (see "Write a new entry" above). Title: "Journal initialized". The entry should document that the journal system was set up and describe the project briefly (inferred from README or directory name).

6. Confirm with: the journal path created, that `docs/journal/GUIDE.md` was written and `@import`ed into `CLAUDE.md`, and the first entry filename.

## Style Rules

- Entries are factual records, not conversational. Write in past tense ("Added...", "Moved...", "Decided...").
- Tables for structured data (file lists, comparisons, pinouts, decisions with rationale).
- Bullet lists for sequential steps or findings.
- Keep summaries to 1-3 sentences.
- Reference other docs with backtick-quoted relative paths (e.g., `docs/hardware.md`).
- Do not duplicate large blocks of content from other docs — reference them.

## Important

- Always use the Read tool to check existing entries before creating duplicates.
- Never overwrite an existing journal entry — sequence numbers are append-only.
- When writing an entry after a work session, capture decisions and rationale, not just "what files changed."
