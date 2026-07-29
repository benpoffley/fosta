---
title: "CLAUDE.md (see AGENTS.md)"
nav_order: 99
layout: default
---

# CLAUDE.md

This file exists only because some tools (Claude Code) look for `CLAUDE.md` specifically and do not yet read `AGENTS.md`.

**The full, authoritative AI agent instructions for Fosta live in [`AGENTS.md`](AGENTS.md).** Read that file — it contains the stack, hard constraints, architectural patterns, and where to find everything else in this wiki.

`AGENTS.md` is the open, cross-tool standard and is kept as the single source of truth. This file is a redirect, not a duplicate — do not add content here that isn't also in `AGENTS.md`.

## Fetching this wiki — important for any AI tool

If you are fetching wiki pages by URL, use the raw GitHub URL, not the Pages site:
```
https://raw.githubusercontent.com/benpoffley/fosta/main/[path-to-file]
```
The `benpoffley.github.io/fosta/...` domain is served through a CDN that can return stale cached content to automated fetchers. See `AGENTS.md` for full detail.
