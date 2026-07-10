---
title: "Sort"
parent: "Views"
nav_order: 2
layout: default
---

# Sort / Curate

**Status:** Decided  
**Name:** TBD — candidates: Sort, Curate

## What it is

The triage and organisation view. A session activity — not ambient navigation. The user enters Sort to process their inbox, apply tags, and create structure. It is distinct from the Layer file navigator (which is ambient browsing).

## Layout

Two panels:
- **Left:** Inbox grid — notes waiting to be sorted
- **Right:** Tag-frequency bubbles — the user's tag vocabulary, sized by usage

## Inbox grid

Notes that have arrived in `Inbox/` and haven't been sorted yet. Grid layout with note cards.

**Interaction model:** TBD — deferred until the drag-to-tag model is fully resolved. See open questions.

## Tag-frequency bubbles

Tags displayed as bubbles, sized by frequency (computed from SQLite `note_tags` count at query time — not stored). Larger bubble = more notes carrying that tag.

Tags are flat, single-level. No hierarchy. Any differentiation (pinned tags, sizing) is a UI-layer concern only.

## Create Track from tag

Hovering any tag bubble surfaces a "Create Track" affordance. Clicking opens Track's creation modal pre-filled with that tag as the population criteria. The user can adjust before confirming.

This is a Global Action — it works on any tag anywhere it's rendered, not just in Sort. See `../foundations/global-actions.md`.

## Open questions

- **Inbox interaction model:** single-click behaviour (select? tag? preview?) — deferred until drag-to-tag model is resolved
- The drag-to-tag gesture direction is not yet decided

## Out of scope

- Tag templates — explicitly out of v1 scope
- Tag co-occurrence connection lines — downgraded to "maybe later," value unproven

## Reference wireframe

![sort wireframe]({{ site.baseurl }}/assets/wireframes/sort.svg)

*Reference wireframe — inbox grid and tag-frequency bubbles. Larger bubble = more notes carrying that tag. Recreated from early hand sketches, June 2026 — reference only, not final UI.*

