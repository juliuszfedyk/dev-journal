# 004 — Fix plugin installation

**Date:** 2026-03-13
**Scope:** plugin packaging, installation

## Summary

Added `.claude-plugin/marketplace.json` manifest and updated README installation instructions to use the correct marketplace plugin commands.

## What changed

| File | Change |
|------|--------|
| `.claude-plugin/marketplace.json` | Created — marketplace manifest with plugin name, owner, and source mapping |
| `README.md` | Replaced installation section with `/plugin marketplace add` and `/plugin install` commands |

## Context

The previous installation instructions referenced commands that don't match the actual Claude Code plugin system. The marketplace.json manifest is required for the plugin to be discoverable and installable via `/plugin marketplace add`. Without it, users couldn't install the plugin from GitHub.
