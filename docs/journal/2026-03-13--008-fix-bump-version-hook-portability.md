# 008 — Fix bump-version hook portability

**Date:** 2026-03-13
**Scope:** `.claude/hooks/bump-version.sh`

## Summary

Fixed the auto-bump-version pre-tool-use hook to work on Windows/Git Bash by replacing non-portable commands (`grep -oP`, `jq`) with POSIX-compatible alternatives (`sed`).

## What changed

| File | Change |
|------|--------|
| `.claude/hooks/bump-version.sh` | Replaced `grep -oP` (PCRE) with `sed -n 's/.../\1/p'` for version extraction (2 occurrences) |
| `.claude/hooks/bump-version.sh` | Replaced `jq` JSON parsing with `sed` for extracting the command field from hook input |

The hook now uses only POSIX-compatible tools: `sed`, `cut`, `grep -q`, `git`.

## Context

The hook was created in an earlier session but used `grep -oP` (a GNU/PCRE extension not available in Git Bash on Windows) and `jq` (not installed on this system). Both caused immediate failures when the hook fired. The `.claude/settings.json` hook configuration was already correct and needed no changes.
