# 005 — Added journal help subcommand

**Date:** 2026-03-13
**Scope:** skills/journal/SKILL.md

## Summary

Added a `/journal help` subcommand that displays a command reference table to users. This completes the set of journal commands with a self-documenting help option.

## What changed

| Location in SKILL.md | Change |
|----------------------|--------|
| argument-hint (line 4) | Added `help` to the command list |
| Command table | New row: `/journal help` — Show available commands and usage |
| Subcommand guard list | Added `help` to prevent it from being treated as an entry title |
| New `### help` section | Inserted before `### check` with formatted command reference table |
| README template in `init` | Added `For help: /journal help` line; fixed `/journal write <title>` → `/journal <title>` |

## Context

The journal skill lacked a built-in way for users to discover available commands. The help subcommand provides a quick reference without needing to read the SKILL.md source. This was implemented as part of a planned enhancement to improve discoverability.
