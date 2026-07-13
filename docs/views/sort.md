---
title: "Sort / Curate"
parent: "Views"
nav_order: 2
layout: default
---

# Sort / Curate

**Status:** Decided — name TBD (Sort vs Curate, deferred to hi-fi Figma phase)

## What it is

The triage and organisation view. A deliberate session activity — not ambient navigation. The user enters Sort specifically to process their inbox, apply tags, and create structure. Think of a filmmaker sitting down to review and label a day's footage.

It is distinct from the Layer file navigator, which is ambient browsing. Sort is where you go to process, not to browse.

---

## Layout

Two panels side by side:

| Panel | Contents |
|---|---|
| Left | Note grid — inbox or all notes, with search, tabs, sort and view controls |
| Right | Tag cloud — the user's full tag vocabulary, sized by frequency |

**Single search bar** at the top of the left panel. Typing filters the note grid on the left and simultaneously highlights matching tags on the right. There is no separate search bar for tags — the single bar serves both panels.

This was a deliberate decision: two search bars (one per panel) felt duplicated and unclear. The tag cloud reacts to search rather than having its own independent search.

---

## Left panel controls

| Control | Location | Behaviour |
|---|---|---|
| Search bar | Top of left panel | Filters notes + highlights tags simultaneously |
| Inbox / All tabs | Below search | Inbox = unsorted notes only; All = full vault |
| Sort | Tab row, right | Cycle sort order (date, title) |
| View toggle | Tab row, right | Grid view ↔ list view |

---

## Tag cloud (right panel)

Tags displayed as organic bubbles, sized by usage frequency — computed from SQLite `note_tags` count at query time, not stored. Larger bubble = more notes carrying that tag.

Tags are flat and single-level. No hierarchy. The frequency sizing is the only visual encoding of importance.

The right panel has **no controls of its own**. It reacts to what is happening in the left panel.

---

## Interaction model

The tag cloud operates in two distinct modes depending on whether notes are selected.

### Browse mode (no notes selected)

| Action | Result |
|---|---|
| Click a tag bubble | Filters the note grid to notes carrying that tag. Cream ring appears on the active tag. |
| Click same tag again | Clears the filter |
| Click ✕ pill | Clears the filter |
| Hover a tag | "Create Track" tooltip appears — clickable to create a Track pre-filled with that tag |

### Selection mode (one or more notes selected)

Single-clicking a note card selects it. Multiple cards can be selected. Once a selection exists, the tag cloud switches to tagging mode — tags become apply/remove controls rather than filters.

**Three-state tag toggle:**

Each tag bubble can be in one of three states relative to the current selection:

| State | Visual at rest | Hover visual | Click action |
|---|---|---|---|
| **None** — no selected notes have this tag | No ring (default) | Sage ring + "+" suffix | Add tag to all selected notes |
| **Partial** — some selected notes have it | Cream dashed ring | Amber ring + "–" suffix | Extend tag to remaining notes |
| **Full** — all selected notes have it | Cream dashed ring | Rose ring + "✕" suffix | Remove tag from all selected notes |

**Colour language:**
- **Cream ring (at rest)** — neutral state signal: "this tag is applied to your selection." Not an action colour.
- **Sage** — add (positive action)
- **Amber/gold** — extend to remaining (completing something)
- **Rose** — remove (destructive action)

Action colours only appear **on hover** — never at rest. This prevents misreading the ring state as an action intent.

**Card indicators in partial state:**
When hovering a partially-applied tag, each selected note card shows a small indicator:
- ✓ — this note already has the tag
- + — this note will receive the tag on click

This gives the user a clear preview of what will change across all selected notes before committing.

**Clearing selection:** press Escape or click the grid background.

---

## Sort inbox interaction model

Single-click in the Sort inbox grid is **TBD** — deferred until the interaction design is fully resolved. The selection model described above covers what happens once a note is selected, not how that selection is initiated. Current prototype uses single-click to select; this is provisional.

---

## Create Track from tag

Hovering any tag bubble (in browse mode) surfaces a "Create Track" tooltip. Clicking "Create Track" opens Track's creation modal pre-filled with that tag as the first population criteria.

The tooltip uses a grace period (280ms) and an invisible hover bridge so the user can move their mouse to the tooltip and click it without it disappearing. See [Global Actions](../foundations/global-actions.md).

---

## Reference wireframe

<img src="/fosta/assets/wireframes/sort.svg" alt="Sort wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — inbox grid and tag-frequency bubbles. Recreated from early hand sketches, June 2026. Not final UI.*

---

## Design prototype

An interactive HTML prototype explores the Sort layout and interaction model. It is a reference for the hi-fi Figma design — not a locked specification.

**[View Sort prototype](/fosta/assets/prototypes/sort-v1.html)**

Key decisions explored in the prototype:
- Single search bar filtering both notes and tags simultaneously
- Select-then-tag interaction model (select notes, then click tag to apply/remove)
- Three-state tag toggle (none / partial / full) with hover-reveal action colours
- Card ✓/+ indicators in partial state
- Tag filter mode vs tagging mode colour distinction (cream = applied state, not action)
