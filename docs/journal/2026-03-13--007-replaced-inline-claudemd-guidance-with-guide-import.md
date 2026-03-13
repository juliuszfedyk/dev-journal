# 007 — Replaced inline CLAUDE.md guidance with GUIDE.md @import

**Date:** 2026-03-13
**Scope:** SKILL.md init flow, docs/journal/GUIDE.md

## Summary

Refactored the `/journal init` command to write journal guidance into a separate `docs/journal/GUIDE.md` file and reference it from `CLAUDE.md` via `@docs/journal/GUIDE.md`, instead of inlining the guidance text directly into CLAUDE.md.

## What changed

| File | Change |
|------|--------|
| `skills/journal/SKILL.md` | Init step 3: now writes `docs/journal/GUIDE.md` with guidance content |
| `skills/journal/SKILL.md` | Init step 4 (new): appends `@docs/journal/GUIDE.md` to CLAUDE.md instead of inlining text |
| `skills/journal/SKILL.md` | Steps 4→5 and 5→6 renumbered accordingly |
| `skills/journal/SKILL.md` | Help command and command table updated to mention GUIDE.md |
| `skills/journal/SKILL.md` | Confirmation message updated to mention GUIDE.md and @import |
| `docs/journal/GUIDE.md` | Created as a reference copy of the guidance content |

## Context

Using `@import` keeps CLAUDE.md concise and avoids duplicating guidance text inline. The guidance content lives in `docs/journal/GUIDE.md` alongside the journal entries it describes, and CLAUDE.md simply references it. This follows the Claude Code `@import` convention for modular project instructions.
