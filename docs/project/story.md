---
title: "Story"
parent: "Project"
nav_order: 6
layout: default
---

# Story

**Role:** the narrative record of *why* decisions were made. For a terse list of *what* changed and *when*, see [Changelog](changelog.md).

A log of how Fosta was built — decisions made, problems solved, things learned. Most recent first. The story begins in February 2026; the June 2026 wiki entries below consolidated foundational decisions first explored in that earlier period.

---

## Settings defined as its own Foundation; vault switcher separated from it
**8 August 2026 · Architecture decision**

Fosta gains a proper global Settings surface, accessed via the standard macOS `Fosta → Settings…` menu (⌘,) rather than a bottom-toolbar icon — keeping the toolbar's identity as the view switcher plus Capture intact.

A deliberate split was made between vault switching and Settings, even though both initially seemed like the same feature. Vault switching is something a user might do many times a day; Settings is touched rarely. Bundling them would bury a frequent action inside a rare-action menu. The vault switcher is instead a persistent, always-visible control in the top-left of the window — exact visual treatment deferred to the hi-fi Sort Figma pass, where it will be designed as part of the same visual system.

A general pattern emerged and was applied consistently to both vaults and Desk's wipe-frequency options: **quick-add lives at the point of use, edit/delete lives in Settings.** Adding a new vault or a new custom wipe frequency is safe and additive, so it's exposed directly in the vault switcher dropdown and the Timer pill's frequency dropdown respectively, with no detour required. Renaming or removing a vault, and editing or deleting a custom wipe frequency, are Settings-only — actions that could affect something already depended on get deliberate friction, actions that can't break anything don't.

Two things were explicitly confirmed as out of scope: no wipe-confirmation prompt (all wipe behaviour stays in Desk's Timer pill, unchanged), and no user accounts in v1 (no login, no sign-out, no cross-device sync — work-access is a device-level pairing, not an identity system).

---

## Multi-vault support decided; view state persistence gap discovered and filled
**8 August 2026 · Architecture decision**

Fosta will support multiple independent vaults, switchable via a global selector — each vault a fully sealed folder with its own notes, SQLite index, and view state. The key insight that made this straightforward rather than heavy: since a vault's contents were always going to live in a user-chosen folder anyway, the only real requirement is that `StorageAdapter` takes its root path as runtime configuration rather than a fixed value — something worth building correctly from day one regardless, since retrofitting it later would touch every call site that assumed a single implicit vault.

Cloudflare work-access (ADR-010) is deliberately scoped to exactly one designated vault for v1, rather than syncing every vault or building vault-selection into the capture flow immediately — kept simple until there's real evidence multiple users need simultaneous remote capture into more than one vault. Multi-vault sync and capture-time vault selection are explicitly left open for v2, with no architectural changes required to add them later.

While writing this up, a real gap was found: an earlier session had worked through view state persistence in detail — per-view state, Sort's deliberate reset-to-Inbox exception, the rule that explicit navigation only overrides "what's open" and never a view's configuration, Track gaining tabs, and per-layout-mode default zoom behaviour on reopen (zoom-to-fit for Web, fixed zoom at the working end for Line, scroll-to-top for Thread) — but none of it had actually been written into the wiki. This is now documented as its own Foundations page, View State Persistence, and cross-referenced from the new multi-vault ADR.

One question surfaced during that write-up remains genuinely open and is now logged: what Track's file navigator actually browses when open — notes, other Tracks, or both via a mode toggle.

---

## Freeform node timestamps — automatic, no manual dating required
**14 September 2026 · Design decision**

Freeform nodes on Desk now silently record their creation moment — the instant the first character is typed — with no user action required. This is the node's only timestamp for as long as it stays freeform; `modified` has no meaning until a node becomes a tracked note, so freeform nodes simply don't carry that field.

Shown subtly: low-opacity, visible only on hover or when selected, keeping Desk's blank-canvas feel intact.

On "Capture to Inbox," the freeform node's original timestamp becomes the new note's `created` date — preserving the idea's true origin even if it sits on the canvas for days before being captured. `modified` is set to the moment of capture and updates normally from there. Pinning a node does not add a second "pinned on" timestamp — only the original creation date is tracked.

Schema: `createdAt` added to the JSON Canvas node object, present on `freeform` nodes only.

---

## AI agent entry point migrated to AGENTS.md; raw GitHub URL guidance added
**23 July 2026 · Infrastructure**

Two related fixes. First: a stale-cache issue was discovered where AI tools fetching wiki pages via the GitHub Pages URL (`benpoffley.github.io/fosta/...`) could receive old cached content even when the live site was current — confirmed by comparing a hard-refreshed browser view (current) against a tool fetch (stale, showing pre-rename "Linear/Spatial" Track content from weeks earlier). The fix: any AI tool should fetch wiki content via the raw GitHub URL (`raw.githubusercontent.com/benpoffley/fosta/main/...`) instead, which bypasses the Pages CDN entirely. This guidance is now documented at the top of the agent instructions file itself, so it travels with the wiki.

Second: `CLAUDE.md` was renamed to `AGENTS.md`, adopting the open, cross-tool standard (Linux Foundation-governed, read natively by Cursor, Codex, Copilot, Gemini CLI, and others) rather than the Anthropic-specific convention. A minimal `CLAUDE.md` pointer file remains, since Claude Code specifically still only reads that filename and does not yet read AGENTS.md natively. All other wiki references updated to point to AGENTS.md as the source of truth.

---

## Comment visibility rules made explicit
**23 July 2026 · Clarification**

A gap was identified: comments were defined as full notes (`type: comment`) per "everything is a note," but the wiki never explicitly stated whether comments should appear in the file navigator, Sort's inbox/tags, or search — leaving this to inference.

Resolved with an explicit visibility table in the Layer view page. Comments remain full notes architecturally — UUID, frontmatter, disk file, SQLite-indexed, no parallel object type — but are excluded from every view that presents "notes" for browsing, tagging, or referencing (file navigator, Sort inbox/tags, Quick Look, canvas note-reference nodes). The one exception is search: comments are real content and should be findable, even though they aren't meant to be browsed as standalone notes. The canonical access point for a comment remains the parent note's Layers panel.

---

## Pinned nodes introduced on Desk — reopens and refines a previously rejected idea
**23 July 2026 · Design decision**

Desk gains a "pinned" property, available on any canvas node (freeform, note-reference, canvas-reference). A pinned node is excluded from the timer's wipe cycle and persists on the canvas indefinitely, in place, until unpinned.

This directly revisits an earlier rejected concept — dedicated Focus and Pinned side panels on Desk, rejected on the grounds that a fixed UI region undermines Desk's core promise as a blank, chrome-free canvas. The new approach avoids that problem entirely: pinning is a property on ordinary canvas content, not a new UI surface. A pinned node looks and behaves exactly like its unpinned counterpart — same position, same two-state model, same interactions — except it survives wipes.

The most useful consequence is for freeform nodes specifically. A freeform node — plain text typed directly onto the canvas, with no underlying note — can now be pinned to create a persistent list or set of reminders, edited directly in place, without ever being converted to a real note via Capture to Inbox. This gives users a lightweight, always-available scratch list without reopening the "everything is a note" principle: unpinned freeform nodes were already the one accepted exception to that rule, and pinning simply extends their lifecycle rather than creating a new exception.

Schema: `pinned` boolean added to the JSON Canvas node object, default `false`, Desk-only behaviour (no effect on Track canvases, which have no wipe cycle).

---

## Sort view design completed through interactive prototyping
**16 July 2026 · Design milestone**

The Sort view interaction model was fully worked out through a series of iterative HTML prototypes, resolving every major open question and producing a design ready for hi-fi Figma work.

**Layout settled:** a shared top bar spans the full width with the "Notes" and "Tags" panel labels prominent in large Playfair italic, and the search bar floating centred over the vertical dividing line — which runs the full height of the view. The two panels are clearly divided but the search visually belongs to both.

**Tag visual treatment:** tags are pure glowing orbs — radial gradients with no stroke or hard edge at rest. Strokes only appear when a tag is actively selected or being hovered with an action intent, keeping the panel calm and uncluttered by default. Bubble sizing by frequency was retained; a flat grid alternative was explored and rejected.

**Multi-tag filter:** multiple tags can be active simultaneously (AND logic). Cream ring signals "active filter" — distinct from the action colours (sage/amber/rose) which only appear on hover.

**Selection model:** single-click selects notes. Selection persists after applying a tag, so the user can apply multiple tags in one session without re-selecting. Three-state toggle (none/partial/full) handles multi-note selections gracefully, with card ✓/+ indicators previewing partial-state changes before commit.

**Inbox management:** "Clear sorted" button slides up at the bottom of the notes panel when tagged notes exist. Staggered exit animation. Auto-clears on navigation away — inbox is a display filter, not a folder.

**Tag creation:** two paths — "+ New tag" button (inline input slides in from top of tag panel) and search-to-create (type a non-existent name, create row appears). Both normalise names on creation.

Prototype linked from Sort view page. Sort is now ready for hi-fi Figma.

---

## Sort interaction model resolved through prototyping
**13 July 2026 · Design**

The Sort view interaction model was worked through in detail via an interactive HTML prototype, resolving several open questions that had been deferred since the early wireframe phase.

The key decisions: a single search bar at the top of the left panel replaces the two-search-bar approach sketched in early wireframes — the bar filters notes on the left and simultaneously highlights matching tags on the right. The tag cloud has no independent controls.

The select-then-tag model was established: single-clicking a note selects it, and once a selection exists the tag cloud switches mode — tags become apply/remove controls. A three-state toggle (none/partial/full) handles multi-select gracefully, with the partial state surfacing ✓/+ indicators on each card so the user can see exactly which notes will be affected before committing. Colour language was carefully separated: cream rings signal "this tag is already applied" (a state), while sage/amber/rose reveal on hover only when the user is actively deciding on an action (add/extend/remove). The filter mode uses cream too but is mutually exclusive with tagging mode.

The prototype is linked from the Sort view page as a reference for the hi-fi Figma design.

---

## Track gains a third layout mode — Line, Web, Thread — plus population model finalised
**9 July 2026 · Milestone**

Track's two modes (Linear, Spatial) renamed to Line and Web, and a third layout — Thread — added: full note content stacked vertically and scrollable, sharing Line's population/sort logic but with no canvas rendering, no connections, and no annotations. Read-only in v1; inline editing deferred to v1.1.

The population model was formalised across all three layouts: every Track is Manual or Live. Live tracks are driven by a fixed, AND-only combination of tags set at creation — a note appears if and only if it carries every tag in the combination. Live tracks have no "+ add" action and no file navigator; the only item-creation action is "Create new," which pre-tags a new note with the Track's full combination. Because the note is born fully tagged, it skips Inbox (a display consequence, not a change to file routing). Switching a Live track to Manual is one-way, freezing its membership.

File navigator visibility was scoped to Manual tracks only (any layout) — a deliberate reversal of the original "no navigator in Track" rule, kept narrow: Live tracks retain the original constraint, avoiding the ambiguity a drag-and-drop add would create against an automatic population rule.

Thread storage was decided as JSON Canvas on disk (same format as Line and Web), with tldraw not invoked for rendering — the scroll UI reads the node list directly. This keeps one storage model for all three Track layouts.

A zoom-model alternative was explored (Line and Thread as one view at different zoom levels) and consciously deferred to post-v1 — see Future Ideas.

---

## Global Actions and Quick Look established as Foundations-level patterns
**7 July 2026 · Milestone**

Two decisions resolved in one session, both documented under Foundations.

Global Actions formalised with two v1 implementations: Add to Desk (sends any note or Track canvas to the Desk scratchpad as a reference node via overflow menu or direct button depending on context) and Create Track from tag (hover any tag anywhere it renders to open Track's creation modal pre-filled). The UI pattern decision was deliberate: notes use overflow because their action set will grow; tags use direct hover-reveal because they are small, single-action elements where overflow adds friction. Long-press parity for mobile documented from the start.

Quick Look formalised as the canonical term for the floating preview modal, superseding all prior names. The connection to Capture's expanded state was made explicit — they are the same surface. Two variants defined: editable note variant (title + body, autosave via saveNote()) and read-only Track canvas variant. Wikilink navigation within Quick Look establishes a session-only back-arrow stack. The interaction model settled as single click selects, double click opens Quick Look, explicit overflow or button escalates to full native view.

The canvas node two-state model was defined: compact card vs expanded live preview triggered by dragging past a size threshold that snaps the node into live content mode. Applies consistently to Desk and Track canvases for both node types.

---

## Canvas-reference nodes added to Desk scratchpad
**7 July 2026**

A third node type added to the Desk scratchpad: canvas-reference nodes, pointing to a Track canvas file by UUID and rendering a live, read-only preview using tldraw. Not editable from Desk.

---

## Original sketches recreated as clean reference wireframes
**18 June 2026**

Hand-drawn notebook sketches redrawn as schematic wireframes and embedded in the wiki. Early reference points, not final UI.

---

## First wireframe sketches reviewed — six decisions made
**17 June 2026**

Hand sketches reviewed. Six decisions resolved: Layer naming deferred to hi-fi, Sort links = wikilinks, tag templates out of v1, tags flat, Share three-pane, Global Actions principle established.

---

## Track redefined as constrained, not free-form
**16 June 2026**

Track explicitly repositioned as a constrained timeline/relationship tool, not a free-form canvas. Two layout modes: Linear (sequence axis) and Spatial (mindmap). Connections always between exactly two nodes, visual-only. Annotations anchor to exactly one node or connection — never float freely.

---

## Smart Paste upgraded: live transclusion in v1
**12 June 2026**

Smart Paste (⌘V into Share) upgraded from creating snapshots to creating live QuoteBlock transclusions. Drag-to-quote and Smart Paste both coexist. On publish, transclusions freeze to snapshot_text.

---

## Product wiki created
**12 June 2026**

First version of the product wiki built. Single HTML file covering product brief, philosophy, architecture, data model, and all views.

---

## Core product philosophy articulated
**12 June 2026**

"All views show the same data through different lenses" — this became the foundational principle. The user's mode of working determines which view they're in, not which data they can access.

---

## Layer view architecture decided
**12 June 2026**

Three-panel layout finalised: collapsible file navigator left, Tiptap block editor centre, collapsible layers panel right. Block UUIDs permanent, serialised as HTML comments. Block-level comments are notes with type: comment.

---

## Competitive landscape mapped
**12 June 2026**

Obsidian, Notion, Bear, Capacities, Craft, Apple Notes reviewed. Key insight: Fosta's differentiation is workflow (pipeline) + local-first + creative professional focus, not feature count.

---

## Capture bar component design decided
**11 June 2026**

Capture bar is persistent on Desk, transient on all other views. Animates in from + button, collapses on idle, stays open once typing begins.

---

## Base / Scratchpad finalised
**11 June 2026**

Timer pill (24h default, wipe/history/frequency). Two node types (became three with canvas-reference nodes later). Archives to Scratchpad/Archive/YYYY-MM-DD-HHmm.canvas.

---

## Lo-fi wireframes complete
**11 June 2026**

Lo-fi wireframes completed for all views. Six SVG wireframes recreated from hand sketches.

---

## Work-access architecture solved via Cloudflare
**11 June 2026**

Browser form → Cloudflare Worker → D1 → Mac polls → Inbox/. Read-only vault snapshot via R2. No editing at work. One-way flows eliminate sync conflicts. Accepted as a known compromise.

---

## Capture resolved as a global action, not a tab
**11 June 2026**

Capture moved out of the view hierarchy entirely. It is a Foundations-level global action, not a view. The persistent bottom toolbar has five tabs, not six.

---

## Canvas architecture decided
**11 June 2026**

JSON Canvas as on-disk format. tldraw as renderer only. Thin adapter between them. tldraw's internal format never written to disk. (ADR-006, ADR-011)

---

## Core architecture locked
**11 June 2026**

All nine architecture principles finalised: StorageAdapter pattern, UUIDs as identity, everything is a note, references not copies, async everywhere, offline-first, YAML frontmatter required, block UUIDs permanent, JSON Canvas for canvases.

---

## The product brief
**11 June 2026**

Fosta began with a clear product thesis: notes should flow through a pipeline — capture fast, sort later, develop in layers, sequence, then publish. The DaVinci Resolve page-based layout became the structural inspiration. Five views, one bottom toolbar, each view a different mode of working on the same underlying data.

---

## Dedicated workspace set up as work resumed
**17 April 2026**

After the initial exploration, a dedicated "fosta studio" workspace was set up to carry the project forward — the home for the design and strategy work that would later be consolidated into the June wiki.

---

## Early concept explored
**February–March 2026**

The foundational shape of the product was worked out through an extended exploration of how to develop design and strategy with an AI tool as a thinking partner. The decisions that still anchor Fosta today were made here: targeting the Mac platform, the five-view model, and the overall flow of notes through those views. The pipeline metaphor and the "non-linear editor for thought" positioning both date from this period. Much of what the June wiki later formalised was first discovered in these early conversations.

---

## Genesis
**23 February 2026**

Fosta began — the first concept work, in a dedicated Claude project, exploring the idea of a note-taking and idea-development app shaped around how creative work actually flows. The starting question was simple: what would it look like to build a single tool for the whole arc from raw capture to finished, published thought, instead of stitching it together across many apps? `fosta.studio` was secured early.
