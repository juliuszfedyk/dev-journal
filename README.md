# dev-journal

A Claude Code plugin that adds a `/journal` skill for development journaling — tracking decisions, sessions, and architecture changes per-project.

## Commands

| Command | Description |
|---------|-------------|
| `/journal` | Write a new entry (default) — summarizes the current session |
| `/journal <title>` | Write a new entry with a specific title |
| `/journal search <query>` | Search entries by filename, then full text |
| `/journal last [N]` | Show the last N entries (default 1) |
| `/journal help` | Show available commands and usage |
| `/journal check` | Find entries relevant to the current conversation |
| `/journal init` | Set up the journal directory, README, CLAUDE.md guidance, and first entry |

## Features

- **Proactive context surfacing** — `/journal check` is designed to be invoked automatically when starting work or before architectural decisions, not just on explicit command.
- **Auto-init** — Writing your first entry automatically runs `init` if no journal exists yet.
- **CLAUDE.md integration** — `init` adds journal guidance to your project's `CLAUDE.md`, so Claude knows to check past decisions before making new ones.
- **Two-phase search** — Searches filenames first, then falls back to full-text grep with context excerpts.

## Entry format

Entries are stored in `docs/journal/` with the naming convention:

```
YYYY-MM-DD--NNN-short-description.md
```

- `YYYY-MM-DD` — date of the entry
- `NNN` — zero-padded per-day sequence number (resets each day)
- `short-description` — slugified title

Each entry contains a structured template: summary, what changed, and context/rationale.

## Installation

```sh
/plugin marketplace add juliuszfedyk/dev-journal
/plugin install dev-journal@dev-journal
```

## How it works

This plugin contains no runtime code. The entire skill is defined in `skills/journal/SKILL.md` as a prompt template. When invoked, Claude Code follows the instructions in `SKILL.md` using its built-in tools (Read, Write, Glob, Grep) to manage journal entries.

## License

MIT — see [LICENSE](LICENSE).

## Privacy

This plugin collects no data. See [PRIVACY.md](PRIVACY.md) for details.
