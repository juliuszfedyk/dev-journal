# 006 — Removed confirmation step from write flow

**Date:** 2026-03-13
**Scope:** `skills/journal/SKILL.md`

## Summary

Removed the confirmation/review step from the journal write flow so entries are written directly without asking the user to approve the proposed content first.

## What changed

| File | Change |
|------|--------|
| `skills/journal/SKILL.md` | Deleted step 6 ("Show the user the proposed entry content and filename for review before finalizing") and renumbered step 7 → 6 |

## Context

The confirmation step added friction to the journal workflow. Since journal entries are append-only and can always be edited after creation, the review gate was unnecessary overhead.
