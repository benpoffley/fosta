---
title: "Sort / Curate"
parent: "Views"
nav_order: 2
layout: default
---

# Sort / Curate

**Status:** Decided — name TBD (Sort vs Curate, deferred to hi-fi Figma phase)  
**Note:** The interaction model on this page is locked in. Visual styling is still work-in-progress — see [Design prototypes](#design-prototypes) below for the two current explorations.

## What it is

The triage and organisation view. A deliberate session activity — not ambient navigation. The user enters Sort specifically to process their inbox, apply tags, and create structure. Think of a filmmaker sitting down to review and label a day's footage.

---

## Layout

Two panels divided by a continuous vertical rule, sitting below the global [App Header](../foundations/app-header.md) (logo, vault switcher, search, Capture — search here doubles as Sort's own search, see below).

| Panel | Contents |
|---|---|
| Left — Notes | Note grid with tabs, sort dropdown, view toggle, Clear sorted |
| Right — Tags | Tag chips — the user's full tag vocabulary |

Below the App Header, a **sub-bar** holds the two panels' own controls, one row, divided by the same vertical rule:

- **Notes half** (left → right): "Notes" title → Inbox/All tabs → sort dropdown + grid/list toggle → *(when applicable)* Clear sorted, pushed to the far right
- **Tags half** (left → right): "Tags" title → "+ New tag" button → contextual hint text → *(when applicable)* the active filter pill or search-to-create prompt, pushed to the far right

**Both halves read in the same direction** — title, then primary control, then secondary/hint content, then transient state pushed to the far edge. This was corrected during hi-fi prototyping: the Tags half originally mirrored the Notes half (title on the right, controls flowing leftward), which fought the Notes side's direction. They now match.

---

## Search

Search lives in the global [App Header](../foundations/app-header.md), not inside Sort's own layout — it's the same search bar every view shares. On Sort specifically, typing:
- Filters the note grid on the left
- Highlights matching tag chips on the right (non-matching chips fade back)

**Search-to-create:** if the search term doesn't match any existing tag, a compact "Create *[term]* as a new tag" chip appears at the far right of the Tags sub-bar (the same transient-element slot the filter pill occupies — they never show at the same time). Clicking it creates the tag immediately and clears the search.

---

## Left panel controls

| Control | Location | Behaviour |
|---|---|---|
| Search | Global App Header | Filters notes + highlights tags simultaneously |
| Inbox / All tabs | Sub-row, left | Inbox = unsorted notes; All = full vault, unified across content types (see below) |
| Sort dropdown | Sub-row, controls | Shows current sort mode ("↕ Date"); click opens a menu with Date/Title, active one checked. Replaces an earlier blind cycle-button — confirmed during hi-fi prototyping that a single icon with no visible label or alternative wasn't discoverable. |
| Grid / List toggle | Sub-row, controls | Two adjacent, always-visible icon buttons (⊞ grid, ☰ list), active one highlighted. Replaces an earlier single toggle button for the same discoverability reason as Sort above — a toggle hides the option you're not currently in. |
| Clear sorted | Sub-row, far right | Appears inline (not a sliding bottom footer, as earlier documented) when tagged notes exist in the inbox, aligned with the tabs and sort/view controls |

### Responsive note grid

The grid uses `auto-fill` columns with a minimum card width, not a fixed column count. At typical widths it shows 3 columns; as the window widens, it fluidly adds a 4th (and further) column rather than needing an explicit breakpoint. Confirmed during hi-fi prototyping.

### The All tab is a unified content browser, not just notes

Confirmed during hi-fi prototyping: the **All** tab mixes notes, Track canvases, and Desk's Scratchpad archives in one grid, each carrying a distinguishing badge (`TRACK` / `ARCHIVE`). This makes Sort's All tab the closest thing Fosta has to "search and browse everything," not just a note inbox.

- **Double-clicking a Track card** opens that Track directly (in a new tab — see [View State Persistence](../foundations/view-state-persistence.md))
- **Double-clicking an Archive card** opens Desk directly into history mode at that specific archive
- **Inbox tab is unaffected** — it stays notes-only, since its whole purpose is showing the unsorted pile specifically

### Grid view vs. list view — different fields, not just a different layout

These are not the same card reflowed — list view shows different content:

| | Grid view | List view |
|---|---|---|
| Fields shown, in order | Checkbox · date · title · preview text · tags | Checkbox · title · date (hugging the title) · tags |
| Preview / body content | Shown (2-line clamp) | **Removed entirely** — list view is for scanning, not previewing |
| Date position | Its own line, above the title | Inline, directly after the title, before the tags |

Confirmed during hi-fi prototyping: list view's job is dense scanning, so it drops the preview text and pulls the date snug against the title (title truncates with an ellipsis if needed) rather than pushing it to the far edge of the row.

---

## Tag panel (right side)

### Visual model — chips, not a frequency-sized cloud

Tags are shown as **flat chips** in a wrapping grid — each chip shows the tag's name and a count badge (number of notes carrying it). This replaced an earlier direction where tags were glowing orbs sized by frequency, positioned in a spatial cloud. The chip model was chosen deliberately during hi-fi prototyping: it's more legible at volume, doesn't require spatial layout logic, and the count number does the "how frequently is this used" job the orb's size used to do — more precisely, in fact, since a number is exact and a size is only approximate.

Tags are flat and single-level. No hierarchy.

### Order is stable — filtering never reorders the list

Confirmed during hi-fi prototyping: filtering or applying a tag does **not** move it to a different position in the list. An earlier direction had active filters sort to the front of the cloud; this was found to be disorienting — the list would jump around as tags were selected and deselected. A filtered or applied tag now stays exactly where it sits, and only its visual state changes (see Interaction model, below). The active filter is still clearly visible via the filter pill in the sub-bar, so reordering added confusion without adding clarity.

### Tag creation

Two paths to create a new tag:

**"+ New tag" button** (sub-row, next to the "Tags" title) — opens a compact inline input that slides in from the top of the tag panel. Type a name and press Enter or click Create. Escape cancels.

**Search-to-create** — type any name in the header search bar. If it doesn't match an existing tag, a create chip appears at the far right of the Tags sub-bar. Fast path for users who know the pattern.

Tag names are normalised on creation: lowercase, spaces to hyphens.

---

## Interaction model

The tag panel operates in two distinct modes depending on whether notes are selected.

### Browse mode (no notes selected)

| Action | Result |
|---|---|
| Click a tag chip | Adds tag to active filter. Cream/ink ring appears on the chip, in place — no reordering. |
| Click another tag | Added to filter (AND logic — notes must carry all active tags) |
| Click an active tag | Removes it from filter |
| Hover a tag | "Create Track" tooltip appears (clickable) — or "Open live Track" if a Live track already exists for that exact tag, see [Global Actions](../foundations/global-actions.md) |

**Multi-tag filter:** multiple tags can be active simultaneously. The note grid shows only notes that carry **all** active filter tags. A filter pill appears at the far right of the Tags sub-bar showing the active combination (e.g. "Filtered: film + client-x").

### Selection mode (one or more notes selected)

Single-clicking a note card selects it. Multiple cards can be selected. Once selected, the tag panel switches to tagging mode.

**Selection persists after tagging** — clicking a tag applies or removes it but does not clear the selection. The user can apply as many tags as needed in one session.

**Three-state tag toggle:**

Each tag chip can be in one of three states relative to the current selection:

| State | Visual at rest | Hover visual | Click action |
|---|---|---|---|
| **None** — no selected notes have this tag | No ring, default chip | Sage ring + "+" suffix | Add to all selected |
| **Partial** — some selected notes have it | Cream/ink ring | Amber ring + "–" suffix | Extend to remaining |
| **Full** — all selected notes have it | Cream/ink ring, slightly brighter | Rose ring + "✕" suffix | Remove from all selected |

**Colour language:**
- **Cream/ink ring (at rest)** — neutral: "this tag is applied to part or all of your selection" (colour depends on light/dark mode — see Design prototypes)
- **Sage** — add (positive)
- **Amber/gold** — extend to remaining (completing something)
- **Rose** — remove (destructive)

Action colours only appear **on hover** — never at rest.

**Card indicators in partial state:** hovering a partially-applied tag shows ✓ or + on each selected card, previewing which notes will be affected before clicking.

### Clearing a selection or a filter — one consistent method, across both

Confirmed during hi-fi prototyping: selecting notes and filtering tags now clear the **same way**, and only that way:

- Press **Esc**, or
- Click the **✕** on the pill (the selection-status pill for notes, the filter pill for tags)

**"Click outside the notes/tags area to clear" was explicitly removed as a clearing method for both.** It was undiscoverable — nothing signalled that clicking the background would do anything — and inconsistent, since it only ever applied to notes, never to tags. Esc + an explicit ✕ on a visible pill is the one method for both, and the two pills share the same visual treatment (see below) so the pattern reads as one consistent interaction, not two similar-but-different ones.

**The selection-status pill and the filter pill are visually identical** — same neutral outline, same fill, same padding, radius, and hover behaviour. Selecting notes shows "X notes selected · keep clicking tags [✕]" at the far right of the Notes sub-bar; filtering tags shows "Filtered: [tags] [✕]" at the far right of the Tags sub-bar. Same component, same position logic (far right of its own panel's sub-bar), different content.

---

## Inbox management

### Within a Sort session — manual clear

Tagged notes remain visible in the inbox grid during a session. This is intentional: removing notes immediately on tagging would disrupt flow, make it harder to review decisions, and prevent easy undo.

When at least one inbox note has been tagged, a **"✦ Clear sorted (N)"** button appears inline in the Notes sub-bar, far right — aligned with the tabs and sort/view controls (not a sliding bottom footer, as earlier documented). Clicking it animates tagged cards out with a stagger, leaving only genuinely untagged notes. The empty state confirms "Inbox clear — X notes sorted."

### On navigation away — automatic clear

When the user navigates to another view and returns to Sort, the inbox automatically shows only untagged notes. Tagged notes still exist in "All notes" — they have simply graduated out of the triage queue. Inbox is a display filter, not a folder.

---

## Create Track from tag

Hovering any tag chip (in browse mode) surfaces a "Create Track" tooltip — or "Open live Track" if a matching Live track already exists (see [Global Actions](../foundations/global-actions.md)). The tooltip has a 120ms appear delay, a 280ms hover grace period, and an invisible bridge between the chip and the tooltip so the user can move their mouse to click it without it disappearing.

Clicking it opens Track's creation modal pre-filled with that tag as the first population criteria, or opens the existing Live track directly if one already matches.

---

## Reference wireframe

<img src="/fosta/assets/wireframes/sort.svg" alt="Sort wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Early reference wireframe — June 2026. Superseded by the hi-fi prototypes below for layout and interaction; kept for historical reference.*

---

## Design prototypes

The interaction model on this page is locked in — verified against three working prototypes. **Visual styling is not final.** Two current explorations exist side by side, both work-in-progress:

**[Dark mode](/fosta/assets/prototypes/sort-hifi-darkmode.html)** — deep forest green ground, terracotta accent, cream/ink text. Palette drawn from a counseling-site reference (warm, editorial, muted).

**[Light mode](/fosta/assets/prototypes/sort-hifi-lightmode.html)** — warm beige ground, near-black ink, the same terracotta accent deepened for contrast. Palette drawn from an architectural logo reference (Cache Valley Breaking).

Both share the exact same markup and interaction logic — only the CSS colour variables differ. Neither is the final visual system; they exist to test whether the locked-in layout and interaction model reads well in both a dark and a light treatment before a single direction is chosen.

An earlier prototype, **[sort-v1](/fosta/assets/prototypes/sort-v1.html)**, is kept for historical reference — it used the now-superseded glowing-orb tag cloud and a Sort-local header (before Capture/vault/search were consolidated into the global App Header). It does not reflect the current interaction model.

### Everything confirmed across this round of prototyping

- Global [App Header](../foundations/app-header.md) — logo, vault switcher, search, Capture — consistent across views (Desk pending discovery)
- Bottom toolbar is now a floating, detached pill, navigation-only (Capture moved to the header)
- Tag chips (name + count) replace the frequency-sized glowing-orb cloud
- Tag order is stable — filtering/applying never reorders the list
- Selection-status pill and filter pill share one visual treatment and one clearing method (Esc or the pill's ✕ — no click-outside)
- Both panel sub-bars flow the same direction (title → primary control → hint → transient state far right)
- Sort control is a labelled dropdown, not a blind cycle button
- Grid/list toggle is two discoverable icons, not a blind toggle
- Note grid is responsive (auto-fill columns) rather than a fixed 3-column layout
- List view shows a different, denser field set than grid view (no preview text; date hugs the title)
- Clear sorted moved from a sliding bottom footer into the Notes sub-bar, inline with the other controls
