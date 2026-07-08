# Story

A log of how Fosta was built — decisions made, problems solved, things learned. Most recent first.

---

## Global Actions and Quick Look established as Foundations-level patterns
**Week 2, 2026 · Milestone**

Two decisions resolved in one session, both documented under Foundations.

Global Actions formalised with two v1 implementations: Add to Desk (sends any note or Track canvas to the Desk scratchpad as a reference node via overflow menu or direct button depending on context) and Create Track from tag (hover any tag anywhere it renders to open Track's creation modal pre-filled). The UI pattern decision was deliberate: notes use overflow because their action set will grow; tags use direct hover-reveal because they are small, single-action elements where overflow adds friction. Long-press parity for mobile documented from the start.

Quick Look formalised as the canonical term for the floating preview modal, superseding all prior names. The connection to Capture's expanded state was made explicit — they are the same surface. Two variants defined: editable note variant (title + body, autosave via saveNote()) and read-only Track canvas variant. Wikilink navigation within Quick Look establishes a session-only back-arrow stack. The interaction model settled as single click selects, double click opens Quick Look, explicit overflow or button escalates to full native view.

The canvas node two-state model was defined: compact card vs expanded live preview triggered by dragging past a size threshold that snaps the node into live content mode. Applies consistently to Desk and Track canvases for both node types.

---

## Canvas-reference nodes added to Desk scratchpad
**Week 2, 2026**

A third node type added to the Desk scratchpad: canvas-reference nodes, pointing to a Track canvas file by UUID and rendering a live, read-only preview using tldraw. Not editable from Desk.

---

## Original sketches recreated as clean reference wireframes
**Week 2, 2026**

Hand-drawn notebook sketches redrawn as schematic wireframes and embedded in the wiki. Early reference points, not final UI.

---

## First wireframe sketches reviewed — six decisions made
**Week 2, 2026**

Hand sketches reviewed. Six decisions resolved: Layer naming deferred to hi-fi, Sort links = wikilinks, tag templates out of v1, tags flat, Share three-pane, Global Actions principle established.

---

## Track redefined as constrained, not free-form
**Week 2, 2026**

Track explicitly repositioned as a constrained timeline/relationship tool, not a free-form canvas. Two layout modes: Linear (sequence axis) and Spatial (mindmap). Connections always between exactly two nodes, visual-only. Annotations anchor to exactly one node or connection — never float freely.

---

## Smart Paste upgraded: live transclusion in v1
**Week 2, 2026**

Smart Paste (⌘V into Share) upgraded from creating snapshots to creating live QuoteBlock transclusions. Drag-to-quote and Smart Paste both coexist. On publish, transclusions freeze to snapshot_text.

---

## Product wiki created
**Week 1, 2026**

First version of the product wiki built. Single HTML file covering product brief, philosophy, architecture, data model, and all views.

---

## Core product philosophy articulated
**Week 1, 2026**

"All views show the same data through different lenses" — this became the foundational principle. The user's mode of working determines which view they're in, not which data they can access.

---

## Layer view architecture decided
**Week 1, 2026**

Three-panel layout finalised: collapsible file navigator left, Tiptap block editor centre, collapsible layers panel right. Block UUIDs permanent, serialised as HTML comments. Block-level comments are notes with type: comment.

---

## Competitive landscape mapped
**Week 1, 2026**

Obsidian, Notion, Bear, Capacities, Craft, Apple Notes reviewed. Key insight: Fosta's differentiation is workflow (pipeline) + local-first + creative professional focus, not feature count.

---

## Capture bar component design decided
**Week 1, 2026**

Capture bar is persistent on Desk, transient on all other views. Animates in from + button, collapses on idle, stays open once typing begins.

---

## Base / Scratchpad finalised
**Week 1, 2026**

Timer pill (24h default, wipe/history/frequency). Two node types (became three with canvas-reference nodes later). Archives to Scratchpad/Archive/YYYY-MM-DD-HHmm.canvas.

---

## Lo-fi wireframes complete
**Week 1, 2026**

Lo-fi wireframes completed for all views. Six SVG wireframes recreated from hand sketches.

---

## Work-access architecture solved via Cloudflare
**Week 1, 2026**

Browser form → Cloudflare Worker → D1 → Mac polls → Inbox/. Read-only vault snapshot via R2. No editing at work. One-way flows eliminate sync conflicts. Accepted as a known compromise.

---

## Capture resolved as a global action, not a tab
**Week 1, 2026**

Capture moved out of the view hierarchy entirely. It is a Foundations-level global action, not a view. The persistent bottom toolbar has five tabs, not six.

---

## Canvas architecture decided
**Week 1, 2026**

JSON Canvas as on-disk format. tldraw as renderer only. Thin adapter between them. tldraw's internal format never written to disk. (ADR-006, ADR-011)

---

## Core architecture locked
**Week 1, 2026**

All nine architecture principles finalised: StorageAdapter pattern, UUIDs as identity, everything is a note, references not copies, async everywhere, offline-first, YAML frontmatter required, block UUIDs permanent, JSON Canvas for canvases.

---

## The product brief
**Week 1, 2026**

Fosta began with a clear product thesis: notes should flow through a pipeline — capture fast, sort later, develop in layers, sequence, then publish. The DaVinci Resolve page-based layout became the structural inspiration. Five views, one bottom toolbar, each view a different mode of working on the same underlying data.
