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

## Changelog rebuilt — every entry since July had been dumped into one undated block
**18 September 2026 · Process fix**

The changelog's own stated purpose is a terse record of *what* changed and *when*, but every change since the initial `v0.2.0 — July 2026` heading was created had simply been appended to that same block, regardless of when it actually happened — three months of entries with no real dates attached, just a stale month-level label from the first entry ever added under it.

Rebuilt by cross-referencing every changelog line against `story.md`'s own dated entries (which had been kept accurate throughout) and re-grouping into proper per-date sections. Nothing was removed — all 85 existing entries carried over, just correctly dated and grouped. The arbitrary `v0.2.0` / `v0.1.0` / `v0.0` version numbering was also dropped in favour of plain dates, since no version number was ever referenced meaningfully elsewhere in the project.

---

## Sort becomes the home for tag management, not just tag application
**18 September 2026 · Design decision**

Sort gains the ability to rename any tag and delete a tag with no notes attached, directly from the tag panel — not just apply and filter tags, as before. Renaming works on any tag regardless of use, since a tag is just a name and renaming it updates every note carrying it in one safe operation. Deletion is scoped to empty tags only: an in-use tag cannot be deleted from Sort, or from anywhere else — there is no bulk "strip this tag from all its notes" action in v1. Retiring an in-use tag means removing it from its remaining notes first, which naturally brings its count to zero and makes it deletable.

This is a deliberate exception to the quick-add/manage split already established for vaults and Desk wipe frequencies, where "manage" actions are pushed into Settings, one step removed from where the item is used. Tags don't follow that pattern: they're worked with continuously as part of Sort's own core activity, not an occasional app-wide configuration change, so their management stays directly in Sort rather than being pulled into Settings. Settings' own documentation now names this explicitly as an exception, rather than leaving Sort's tag-management functionality unexplained against the general rule stated there.

Both new affordances — a rename pencil, and a delete ✕ that only appears on empty tags — are hover-only and scoped to browse mode, since hovering a tag in selection/tagging mode already has an established meaning (revealing the add/extend/remove action colour).

---

## Process language caught bleeding into spec docs; new standing rule added to AGENTS.md
**18 September 2026 · Process fix**

While reviewing the Sort hi-fi and App Header work, a real problem surfaced: spec pages (`docs/product/`, `docs/views/`, `docs/foundations/`) had been written with process narrative embedded directly in them — phrases like "confirmed during hi-fi prototyping," "this replaced an earlier direction," "was found to be disorienting," "as earlier documented." That language belongs entirely in `story.md`, which already contained the full correct narrative — the problem was duplication, not a missing record. Spec pages should read as pure current-state truth; someone opening one cold shouldn't need to know what came before.

Every affected page (`sort.md`, `app-header.md`, `capture.md`, `settings.md`, `global-actions.md`, `product/views.md`, `design-tokens.md`) was rewritten to strip the comparison/history language, leaving only declarative current-state content. `open-questions.md` and `changelog.md` were correctly left untouched — their entire job is tracking resolution history, so that register belongs there.

A standing rule was added to `AGENTS.md` ("Keep process out of product docs") so this doesn't need to be caught and corrected by hand each time: `product/`, `views/`, and `foundations/` pages get current-state-only language; `story.md` and `changelog.md` get the narrative and the log; `open-questions.md` and `design-tokens.md` are named as legitimate exceptions since they exist specifically to track status. The self-check included: if a sentence only makes sense by reference to what the doc used to say, it doesn't belong there.

While doing this pass, the App Header's "bottom toolbar" subsection was also split out into its own Foundation page, **View Switcher** — the two are separate persistent UI elements with no real dependency between them, and "toolbar" was a vague name for something whose actual job is switching views. Terminology updated across all current spec references; left as "bottom toolbar" in this file's own historical entries, since that's the term that was actually in use at the time.

---

## Sort hi-fi prototyping locks in the interaction model; global App Header introduced
**18 September 2026 · Design milestone**

A full hi-fi prototyping pass on Sort produced a locked-in interaction model and surfaced one architectural decision bigger than Sort itself: the App Header.

**The App Header is now a Foundation, not a Sort-specific layout choice.** Fosta's logo, the vault switcher, search, and Capture were consolidated into one persistent header bar, confirmed consistent across views — with one deliberate exception. Capture moves out of the bottom toolbar (previously a transient + button, always-visible only on Desk) into a single, consistently-placed header button. The vault switcher moves out of its earlier standalone top-left position into the header, next to the logo. The bottom toolbar, now holding only the five view tabs, also became a floating detached pill rather than a bar fixed to the bottom edge. **Desk's treatment is explicitly not decided** — it already has its own persistent inline capture bar built into the canvas, and whether that coexists with or is replaced by the header's Capture button needs its own discovery pass, logged as an open question rather than resolved by extension from Sort.

**Sort's tag cloud became tag chips.** The frequency-sized glowing-orb cloud was replaced with flat chips showing a tag's name and an exact count — more legible at volume, and the count number does the "how often is this used" job more precisely than an approximate orb size ever did. A related fix: filtering or applying a tag no longer reorders the list. An earlier direction sorted active filters to the front, which made the list jump around disorientingly; tags now hold their position and just change visual state in place.

**Selection and filtering were unified into one consistent pattern.** Notes-selection status and the tags-filter status are now the same visual component — a neutral outline pill with a dismiss ✕ — positioned symmetrically (far right of each panel's own sub-bar). Both clear the same two ways: Esc, or the pill's ✕. "Click outside to clear" was deliberately removed from both, since it was undiscoverable and previously only applied to notes anyway, not tags.

**Two discoverability fixes to the sub-bar controls.** Sort order was a blind cycle-button with no visible label; it's now a dropdown showing the current mode with both options listed. Grid/list was a single blind toggle hiding whichever mode you weren't in; it's now two adjacent, always-visible icons. Both panel sub-bars were also corrected to flow the same direction — Tags previously mirrored Notes (title on the right), which fought the Notes side; both now read title → primary control → hint → transient state, left to right.

**List view now shows different fields than grid view, not just a reflowed card.** Preview text is dropped entirely; the row order is checkbox → title → date (hugging the title, not pushed to the far edge) → tags. The note grid itself became responsive — auto-fill columns rather than a fixed 3-column layout, so it fluidly adds a 4th column as the window widens.

**Two work-in-progress visual directions were built and added to the repo** — `sort-hifi-darkmode.html` and `sort-hifi-lightmode.html` — sharing identical markup and interaction logic, differing only in CSS colour variables. Dark mode draws from a warm counseling-site reference; light mode from an architectural logo reference (Cache Valley Breaking), with accent colours deepened for contrast on a light ground. Neither is a final visual system — both exist to stress-test whether the locked interaction model reads well in either direction before one is chosen. The earlier `sort-v1.html` prototype (glowing-orb tags, Sort-local header) is kept for historical reference but no longer reflects the current model.

---

## Deep prototype review, round two: Web redefined, freeform extended, several architectural questions resolved
**14 September 2026 · Architecture decision**

A second, much deeper pass through the same hi-fi prototype (full read of every view's interaction logic, not just structural/naming decisions) surfaced a genuine architectural question and several smaller confirmed findings.

**Track's own definition was slightly wrong, and is now corrected.** The prototype's Web layout was built to fully mirror Desk's capabilities — including unanchored, freely-positioned freeform nodes, directly extending Desk's freeform concept rather than reusing Track's existing (and strictly anchor-required) annotation nodes. Testing this against Line confirmed freeform text has no sensible meaning there — it just appends into the sequence. This forced a real question: is Web even still a Track, once it fully mirrors Desk? Resolved as: yes, but Track's own opening definition needed correcting. Track was described as "explicitly NOT a free-form canvas" — true of Line and Thread, never actually true of Web. What actually unifies the three layouts is **persistence and naming** (a Track is created, named, and kept; Desk is disposable and wipes on a timer), not "constraint." Web is the free-form member of the Track family specifically because it's permanent and named, not because it shares Line/Thread's sequence discipline. A separate "Explore" view was considered and rejected — it would have just been Desk's twin, with no real behavioural difference to justify a fourth view existing.

**Freeform nodes now have two hosts with two different lifecycles.** Desk: ephemeral by default, pinning available as an opt-in third lifecycle option. Track's Web layout: permanent by default, no wipe cycle exists at all, so pinning doesn't apply — every Web node is already effectively "pinned" without needing the property. Timestamps, editing, and "Capture to Inbox" conversion behave identically on both hosts. Right-click was confirmed as an equivalent trigger to double-click for opening the "add freeform item / add from library" menu on empty canvas — worth building both, not just double-click.

**Three things the prototype simplified were deliberately NOT carried into the wiki, since the simplifications were build shortcuts, not reconsidered positions:**
- **Delete behaviour stays as documented** — "if a source note is deleted, references surface a warning rather than silently breaking." The prototype's actual delete function cascades and cleans up silently everywhere (Desk, every Track) with no warning; this was confirmed as a prototyping shortcut, not the desired real behaviour, and needs to be built correctly (warn + orphaned reference, not auto-cleanup) in the real app.
- **Smart Paste keeps its richer, previously-decided form** — ⌘V for a paste-choice menu (live quote vs. plain text), ⌘⇧V for always-plain. The prototype simplified to a single always-live-quote ⌘V action; this was not a reconsideration.
- **Backlinks stay UUID-based**, per the existing wikilink syntax (`[[display-name|uuid]]`). The prototype matched backlinks by title text as a static-data shortcut; the real implementation must match by UUID.

**Smaller confirmed findings, all documented as real decisions:**
- "Create Track from tag" reuses an existing Live track for that exact tag rather than duplicating — the hover tooltip itself reflects this ("Open live Track" vs. "+ Create Track")
- Sort's All tab is a unified content browser — notes, Track canvases, and Desk archives together, each with a distinguishing badge and its own double-click destination
- A brief "A clean desk" message after a Desk wipe, fading out over a couple of seconds rather than the canvas silently going blank

---

## Findings from a hi-fi prototyping session reviewed; Develop confirmed as final view name
**14 September 2026 · Design review**

A substantial hi-fi prototype (five fully interactive views built in Claude Design, with realistic seed data) was reviewed against the wiki to surface what should carry forward as real decisions versus what was a prototyping artefact that shouldn't.

**Confirmed final: the view previously called Layer is now Develop.** The prototype consistently used "Develop" throughout — file names, navigation labels, cross-references — and after working with it directly, this was confirmed as the right name over "Layer" (which had already failed the CTA test) and "Work." This closes one of the three view-naming questions that had been deferred to the hi-fi phase; Desk vs Base and Sort vs Curate remain open.

**Rejected: folders reappeared in the prototype, but this was not a deliberate reversal.** The prototype's Sort and Develop navigators showed folder groups (Inbox/Treatment/Reference/Admin), directly contradicting the earlier decision to drop folder-based organisation in favour of flat tags. Confirmed this was an unintentional prototyping default, not a reconsidered position — folders remain out of Fosta's data model and UI entirely. Noted explicitly in `docs/views/develop.md` so this doesn't quietly resurface without this context in a future prototype.

**Superseded: a Settings icon appeared in the prototype's toolbar.** This predates (and is now superseded by) the recent decision that Settings lives under the macOS `Fosta → Settings…` menu, not a toolbar icon. No wiki change needed — noted here so the discrepancy isn't mistaken for a future reversal.

**Resolved: Track's file navigator content**, previously an open question. The prototype's answer — one combined panel with a "Tracks" section (click to open as a tab) above a "Notes" section (search, with "+" to add to the active Manual track) — was confirmed as the right design and is now documented in `docs/foundations/view-state-persistence.md` and `docs/views/track.md`.

**New rule: Inbox notes are hidden from browsing, but stay findable by search.** Working through *why* an untagged note should or shouldn't appear in Develop's, Track's, and Share's navigators surfaced a genuine distinction: browsable lists show organised (tagged) material by default, since an untagged note has no meaningful place to sit yet — but search still reaches it, since search reflects deliberate intent rather than passive browsing. Sort remains the explicit exception, since showing the unsorted pile is its whole purpose. Documented once in `docs/views/develop.md` and referenced from Track and Share rather than repeated three times.

**Minor addition:** Share's QuoteBlocks now have a documented visual distinction between live (pulsing indicator) and frozen (static label) states, taken directly from the prototype's treatment.

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
