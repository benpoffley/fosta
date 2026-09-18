---
title: "Changelog"
parent: "Project"
nav_order: 7
layout: default
---

# Changelog

**Role:** the terse record of *what* changed and *when*. For the narrative of *why* a decision was made, see [Story](story.md).

Documentation milestones. Records meaningful changes — not every wording edit. Grouped by date, most recent first. Cross-reference [Story](story.md) for the fuller reasoning behind any entry below.

---

## 18 September 2026

- Sort: tags can now be renamed (any tag) and deleted (empty tags only) directly in the tag panel — hover-revealed pencil/✕ icons, browse mode only
- Settings: documented tag management as a deliberate exception to the vault/wipe-frequency quick-add-vs-manage pattern — tags are managed in Sort, not Settings
- AGENTS.md: added standing rule "Keep process out of product docs" — product/views/foundations pages are current-state only, prototyping/decision narrative stays in story.md
- Split View Switcher out of App Header into its own Foundation page; renamed "bottom toolbar" to "View Switcher" across current spec docs
- Stripped process/history language (e.g. "confirmed during hi-fi prototyping") from sort.md, app-header.md, capture.md, settings.md, global-actions.md, product/views.md, design-tokens.md
- New Foundations page: App Header — logo, vault switcher, search, Capture consolidated into one persistent header, consistent across views (Desk pending discovery, logged as an open question)
- Capture moved from a view-dependent bottom-toolbar button to a single header button; Desk's existing inline capture bar left unreconciled pending discovery
- Vault switcher moved from a standalone top-left control into the App Header
- Bottom toolbar (now the View Switcher): floating detached pill, navigation-only
- Sort: tag cloud (frequency-sized glowing orbs) replaced with flat tag chips (name + exact count)
- Sort: tag order is now stable — filtering/applying no longer reorders the list to the front
- Sort: selection-status and filter pills unified into one visual component; clearing standardised to Esc or the pill's ✕ only (click-outside removed)
- Sort: sort control changed from a blind cycle-button to a labelled dropdown; grid/list toggle changed from a blind single button to two discoverable icons
- Sort: both panel sub-bars now flow the same direction (title → primary control → hint → transient state far right)
- Sort: list view shows a distinct, denser field set (no preview; date hugs the title) instead of a reflowed grid card
- Sort: note grid is now responsive (auto-fill columns) instead of a fixed 3-column layout
- Added two work-in-progress visual prototypes: `assets/prototypes/sort-hifi-darkmode.html` and `sort-hifi-lightmode.html` — same markup/logic, different colour palettes, neither final

## 14 September 2026

- Track: redefined around persistence/naming rather than "constraint" — Web is Track's free-form member by design, mirroring Desk's capabilities
- Freeform nodes extended to Track's Web layout (not Line, not Thread), distinct from Track's existing anchor-required annotation nodes
- Canvas Nodes: freeform lifecycle now documented per-host (Desk: ephemeral + pinnable; Web: permanent by default, no pinning concept needed)
- Right-click confirmed as an equivalent trigger to double-click for the add-item menu on empty canvas (Desk, Track Web)
- Global Actions: "Create Track from tag" now documented to reuse an existing Live track for the same tag rather than duplicating
- Sort: All tab confirmed as a unified browser across notes, Track canvases, and Desk archives, each with its own double-click destination
- Desk: added the brief "A clean desk" fade-out message shown after a wipe
- Desk: freeform node timestamps added — creation moment recorded automatically, shown on hover/select, maps to `created` on note capture
- JSON Canvas schema: added `createdAt` field to freeform nodes
- Confirmed NOT changing, despite prototype simplifications: delete-with-warning (not cascade-delete), Smart Paste's full paste-choice menu, UUID-based backlinks
- Layer renamed to Develop across the entire wiki — confirmed as the final view name
- Views naming: Layer vs Work vs Develop resolved as Develop; Desk vs Base and Sort vs Curate remain open
- Track file navigator question resolved: combined Tracks + Notes panel with tabs
- New rule: Inbox notes hidden from default browsing lists (Develop, Track, Share) but findable via search; Sort remains the exception
- Confirmed folders remain out of scope — flagged explicitly so this doesn't quietly resurface
- Share: added visual distinction between live and frozen QuoteBlocks

## 8 August 2026

- ADR-014 added: multi-vault support decided for v1 — swappable StorageAdapter root path, work-access scoped to one designated vault
- ADR-004 and ADR-010 updated with multi-vault addenda
- New Foundations page: View State Persistence — per-view state rules, Sort's exception, explicit-navigation override rule, Track tabs, per-layout default zoom on reopen
- Open Questions: added the question of what Track's file navigator browses (resolved 14 September, above)
- New Foundations page: Settings — global config surface, accessed via Fosta → Settings… (⌘,), separate from the vault switcher
- Vault switcher confirmed as a persistent control, separate from Settings — quick-add in the switcher, manage (rename/remove) in Settings
- Desk: Timer pill's frequency dropdown now reads from an editable list (defaults non-deletable, custom frequencies addable inline, edit/delete in Settings)
- Confirmed out of scope for v1: wipe-confirmation prompts, user accounts/login

## 23 July 2026

- CLAUDE.md renamed to AGENTS.md (open cross-tool standard); CLAUDE.md kept as a minimal pointer for Claude Code compatibility
- Added guidance for AI tools to fetch wiki content via raw.githubusercontent.com rather than the Pages URL, to avoid stale CDN caching
- Layer: comment visibility rules made explicit — full notes architecturally, but excluded from file navigator/Sort/Quick Look/canvas nodes, included in search
- Desk: pinned nodes introduced — any canvas node (including freeform) can be pinned to survive the wipe cycle; refines the previously rejected Focus/Pinned panels concept
- JSON Canvas schema: added `pinned` boolean field to node objects (Desk-only behaviour)
- Canvas Nodes foundation page: added Pinned nodes section

## 16 July 2026

- Sort prototype updated: shared top bar layout, floating search, vertical dividing line, tag orb visual treatment (no resting stroke), multi-tag filter (AND logic), persistent selection for multi-tag apply, Clear sorted button, tag creation (+ New tag button + search-to-create)
- Sort view page fully rewritten with all interaction decisions documented
- ADR-001 expanded with full Tauri decision rationale
- ADR-013 added: platform and mobile strategy — Mac-only v1, mobile deferred, full cloud-first trade-off analysis documented
- ADR index updated with full table of all 13 decisions

## 9 July 2026

- Track: Linear → Line, Spatial → Web; Thread added (vertical scroll, no canvas/connections/annotations, read-only v1)
- Track population model formalised: Manual vs Live (AND-only tag combination), Create-new pre-tagging, Live tracks skip Inbox
- File navigator in Track scoped to Manual tracks only
- Known Compromises: Web mode renamed from Spatial, Thread inline editing added as v1.1 candidate
- Future Ideas page added — Line/Thread zoom model exploration documented for post-v1

## 7 July 2026

- Migrated wiki from single HTML file to multi-file Markdown repo
- Added CLAUDE.md as primary AI agent entry point
- Added build/ layer (BUILD-GUIDE.md, build-sequence.md, file-structure.md, acceptance-criteria.md)
- Added How to Use / order of authority documentation
- Added all 12 Architectural Decision Records (ADR-001 through ADR-012)
- Added AI Guardrails section
- Added Known Compromises section
- Added Current Development section
- Added Global Actions and Quick Look pages under Foundations
- Established Quick Look as canonical term (supersedes "expanded capture card")
- Documented canvas node two-state model (compact / expanded live preview)
- Settled interaction model: single click selects, double click opens Quick Look
- Updated Desk, Layer, Capture docs to reflect the new interaction model
- Restructured nav: Product / Market / Engineering / Foundations / Views / Project

## June 2026

- Initial wiki created — product brief, philosophy, architecture, data model, all views
- Competitive landscape research completed
- Smart Paste upgraded from snapshot to live transclusion — brought into v1 scope
- Track fully specified: Linear and Spatial modes, annotation anchoring model
- Share view updated from two-pane to three-pane layout
- Canvas-reference nodes added to Desk scratchpad
- Reference wireframes recreated from hand sketches
- Capture restructured as a Foundation, not a view
- Global Actions principle established; two v1 implementations documented

## February–April 2026

- Project began 23 February 2026 — first concept work in a dedicated Claude project
- Foundational decisions explored: Mac platform, five-view model, note-flow pipeline
- Pipeline metaphor and "non-linear editor for thought" positioning established
- `fosta.studio` domain secured
- Dedicated "fosta studio" workspace set up 17 April 2026
