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

---

## Layout

Two panels divided by a continuous vertical rule:

| Panel | Contents |
|---|---|
| Left — Notes | Note grid with search, tabs, sort and view controls |
| Right — Tags | Tag cloud — the user's full tag vocabulary as glowing orbs |

**Shared top bar** spans the full width with three zones:
- "Notes" label (left, large Playfair italic)
- Search bar (floating, centred over the vertical dividing line)
- "Tags" label (right, large Playfair italic)

The vertical dividing line runs the full height of the view — through the title row, behind the floating search bar, and through the sub-row below. This makes the two-panel structure immediately clear.

Below the title row, a second row contains:
- Left half: Inbox / All tabs + sort and view toggle controls
- Right half: contextual hint text + "+ New tag" button

---

## Search bar

Single search bar, shared between both panels. Typing simultaneously:
- Filters the note grid on the left
- Highlights matching tag orbs on the right (non-matching tags fade back)

**Search-to-create:** if the search term doesn't match any existing tag, a dashed "Create *[term]* as a new tag" row appears at the top of the tag panel. Clicking it creates the tag immediately and clears the search.

Two search bars were explicitly considered and rejected — the single shared bar is cleaner and makes the relationship between notes and tags explicit.

---

## Left panel controls

| Control | Location | Behaviour |
|---|---|---|
| Search bar | Top bar, centred | Filters notes + highlights tags simultaneously |
| Inbox / All tabs | Sub-row, left | Inbox = unsorted notes; All = full vault |
| Sort | Sub-row, controls | Cycle sort order (date, title) |
| View toggle | Sub-row, controls | Grid ↔ list |
| Clear sorted | Bottom of panel | Slides up when tagged notes exist in inbox |

---

## Tag cloud (right panel)

Tags displayed as glowing orbs, sized by usage frequency — computed from SQLite at query time. Larger orb = more notes carrying that tag.

**Visual treatment:** orbs have no stroke or hard edge at rest. They are pure radial gradient glows that fade to transparent. A stroke only appears when the tag is actively selected (filter mode) or when hovering in tagging mode to signal an action. This keeps the panel calm and uncluttered at rest.

Tags are flat and single-level. No hierarchy.

### Tag creation

Two paths to create a new tag:

**"+ New tag" button** (sub-row, right side) — opens a compact inline input that slides in from the top of the tag panel. Type a name and press Enter or click Create. Escape cancels.

**Search-to-create** — type any name in the shared search bar. If it doesn't match an existing tag, a dashed create row appears in the tag panel. Fast path for users who know the pattern.

Tag names are normalised on creation: lowercase, spaces to hyphens.

---

## Interaction model

The tag cloud operates in two distinct modes depending on whether notes are selected.

### Browse mode (no notes selected)

| Action | Result |
|---|---|
| Click a tag orb | Adds tag to active filter. Cream ring appears. |
| Click another tag | Added to filter (AND logic — notes must carry all active tags) |
| Click an active tag | Removes it from filter |
| Click ✕ pill | Clears all active filters |
| Hover a tag | "Create Track" tooltip appears (clickable) |

**Multi-tag filter:** multiple tags can be active simultaneously. The note grid shows only notes that carry **all** active filter tags. Active tags sort to the top of the cloud. A filter pill in the top-right of the tag panel shows the active combination (e.g. "film + client-x").

### Selection mode (one or more notes selected)

Single-clicking a note card selects it. Multiple cards can be selected. Once selected, the tag cloud switches to tagging mode.

**Selection persists after tagging** — clicking a tag applies or removes it but does not clear the selection. The user can apply as many tags as needed in one session, then press Escape or click the grid background to finish.

**Three-state tag toggle:**

Each tag orb can be in one of three states relative to the current selection:

| State | Visual at rest | Hover visual | Click action |
|---|---|---|---|
| **None** — no selected notes have this tag | No stroke, default glow | Sage ring + "+" suffix | Add to all selected |
| **Partial** — some selected notes have it | Cream dashed ring | Amber ring + "–" suffix | Extend to remaining |
| **Full** — all selected notes have it | Cream dashed ring, brighter glow | Rose ring + "✕" suffix | Remove from all selected |

**Colour language:**
- **Cream ring (at rest)** — neutral: "this tag is applied to part or all of your selection"
- **Sage** — add (positive)
- **Amber/gold** — extend to remaining (completing something)
- **Rose** — remove (destructive)

Action colours only appear **on hover** — never at rest.

**Card indicators in partial state:** hovering a partially-applied tag shows ✓ or + on each selected card, previewing which notes will be affected before clicking.

**Topbar status** shows "X notes selected — keep clicking tags · Esc to finish" while a selection is active.

---

## Inbox management

### Within a Sort session — manual clear

Tagged notes remain visible in the inbox grid during a session. This is intentional: removing notes immediately on tagging would disrupt flow, make it harder to review decisions, and prevent easy undo.

When at least one inbox note has been tagged, a **"✦ Clear sorted (N)"** button slides up at the bottom of the notes panel. Clicking it animates tagged cards out with a stagger, leaving only genuinely untagged notes. The empty state confirms "Inbox clear — X notes sorted."

### On navigation away — automatic clear

When the user navigates to another view and returns to Sort, the inbox automatically shows only untagged notes. Tagged notes still exist in "All notes" — they have simply graduated out of the triage queue. Inbox is a display filter, not a folder.

---

## Create Track from tag

Hovering any tag orb (in browse mode) surfaces a "Create Track" tooltip. The tooltip has a 120ms appear delay, a 280ms hover grace period, and an invisible bridge between the orb and the tooltip so the user can move their mouse to click it without it disappearing.

Clicking "Create Track" opens Track's creation modal pre-filled with that tag as the first population criteria. See [Global Actions](../foundations/global-actions.md).

---

## Reference wireframe

<img src="/fosta/assets/wireframes/sort.svg" alt="Sort wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Early reference wireframe — June 2026. Not final UI.*

---

## Design prototype

An interactive HTML prototype documents the Sort layout and interaction model. Reference for the hi-fi Figma design — not a locked specification.

**[View Sort prototype](/fosta/assets/prototypes/sort-v1.html)**

Decisions explored in the prototype:
- Shared top bar with floating centred search and large panel labels
- Single search bar filtering both panels simultaneously
- Tag orbs as pure glowing gradients with no resting stroke
- Multi-tag filter (AND logic, cream ring, active tags sort to top)
- Select-then-tag model with persistent selection across multiple tag applications
- Three-state toggle (none / partial / full) with hover-reveal action colours
- Card ✓/+ indicators in partial state
- "Clear sorted" button at bottom of notes panel with staggered exit animation
- Tag creation via "+ New tag" button and search-to-create
