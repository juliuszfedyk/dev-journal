# 001 — Journal initialized

**Date:** 2026-03-13
**Scope:** project setup

## Summary

Set up the development journal for the dev-journal plugin repository itself. This dogfoods the `/journal` skill on its own codebase.

## What changed

- Created `docs/journal/` directory with README and this initial entry.
- The plugin was renamed from `journal` to `dev-journal` to better distinguish the plugin name from the `/journal` skill command.

| File | Change |
|------|--------|
| `.claude-plugin/plugin.json` | `name` set to `dev-journal` |
| `README.md` | Heading updated to `dev-journal` |
| `CLAUDE.md` | Description references `dev-journal` plugin |

## Context

The plugin provides a single `/journal` skill defined in `skills/journal/SKILL.md`. Having `docs/journal/` in the plugin repo does not interfere with marketplace distribution — the plugin loader only reads `.claude-plugin/` and `skills/`.
