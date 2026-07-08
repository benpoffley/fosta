# Build Sequence

**Principle:** Build phase by phase. Each phase must be working and in daily use before the next begins.

## Phase 0 — Dev environment (Week 4, Month 1)

- [ ] Install Tauri 2 CLI + Rust
- [ ] Create new Tauri 2 project with React + TypeScript template
- [ ] Configure Tailwind CSS
- [ ] Set up ESLint + Prettier with TypeScript rules
- [ ] Create GitHub repo, first commit
- [ ] Confirm app launches as a native macOS window

## Phase 1 — Data foundation (start of Month 2)

Build the data layer before any UI. Nothing else works without this.

- [ ] Define `Note`, `NoteMetadata`, `StorageAdapter` TypeScript interfaces
- [ ] Implement `LocalFilesAdapter` — list, get, save, delete, watch
- [ ] Implement YAML frontmatter parsing (id, created, modified, tags)
- [ ] Implement UUID generation for new notes
- [ ] Set up SQLite via Tauri SQL plugin
- [ ] Implement vault scanner — reads Markdown files, builds SQLite index
- [ ] Implement file watcher — updates SQLite when files change on disk
- [ ] Write tests: create note → save → reload → confirm frontmatter intact
- [ ] **Gate:** Can create a note via code, find it in the vault, update it, delete it

## Phase 2 — Capture (Month 2)

Build the intake mechanism first. Every other view depends on notes existing.

- [ ] Build `CaptureBar` component (collapsed state)
- [ ] Build `CaptureBar` expanded state (input field, save/discard)
- [ ] Connect to `StorageAdapter.saveNote()` — creates note in `Inbox/`
- [ ] Build Quick Look modal — note variant (title + body, autosave)
- [ ] Wire capture bar → Quick Look modal on expand
- [ ] "Open in Layer" button (Layer not built yet — just logs for now)
- [ ] Build persistent bottom `Toolbar` with five tabs (placeholder views)
- [ ] Capture bar behaviour: persistent on Desk, transient on other views
- [ ] **Gate:** Can capture a note, find it as a Markdown file in Inbox/, see it has correct frontmatter

## Phase 3 — Sort (Months 2–4)

The first real view. Build and use daily before building Layer.

- [ ] Build `FileNavigator` component (read vault, render tree)
- [ ] Build `NoteCard` component (date, title, preview, tags)
- [ ] Build inbox grid (notes where folder = Inbox/)
- [ ] Build `TagBubble` component (size by frequency from SQLite)
- [ ] Compute tag frequency from SQLite `note_tags` table
- [ ] Build "Create Track" hover affordance on `TagBubble`
- [ ] Resolve Sort inbox interaction model (TBD — see open questions)
- [ ] **Gate:** Inbox shows captured notes. Tag bubbles reflect actual tag frequency. Can assign a tag to a note.

## Phase 4 — Cloudflare work capture (Month 4–5)

- [ ] Create Cloudflare Worker (receive POST from browser form)
- [ ] Set up D1 database (staging inbox)
- [ ] Build browser capture form (simple HTML, hosted on Cloudflare Pages)
- [ ] Build Mac polling service (check D1, pull new notes, write to Inbox/)
- [ ] Set up R2 bucket
- [ ] Build vault snapshot generator (Mac → R2)
- [ ] Build read-only vault viewer (R2 → static site)
- [ ] **Gate:** Can submit a note from a browser, see it appear in Mac inbox within polling interval

## Phase 5 — Layer (Months 5–7)

- [ ] Integrate Tiptap editor
- [ ] Configure block types (paragraph, heading, list, code, etc.) with slash command
- [ ] Implement block UUID generation and serialisation as HTML comments
- [ ] Implement UUID preservation through save/load cycle (test explicitly)
- [ ] Build Tiptap → Markdown serialiser (preserving block UUID comments)
- [ ] Build Markdown → Tiptap deserialiser (restoring block UUIDs as node attributes)
- [ ] Build tab bar (multiple open notes)
- [ ] Build collapsible `LayersPanel` — metadata + backlinks
- [ ] Build block-level comments (create `type: comment` note, anchor to block UUID)
- [ ] Implement wikilink `[[display|uuid]]` — click opens Quick Look
- [ ] Build wikilink back-arrow navigation stack in Quick Look
- [ ] **Gate:** Can write a note in Tiptap, save, reload, confirm block UUIDs intact. Can add a wikilink, click it, see Quick Look open.

## Phase 6 — Share (Month 7–8)

- [ ] Build three-panel Share layout
- [ ] Build Share note (`type: share` frontmatter)
- [ ] Build `QuoteBlock` component (renders transclusion content)
- [ ] Implement drag-to-quote gesture
- [ ] Implement Smart Paste (⌘V → QuoteBlock)
- [ ] Implement transclusion resolution from SQLite at render time
- [ ] Implement `snapshot_text` fallback
- [ ] Implement publish (freeze transclusions, set `published: true`)
- [ ] **Gate:** Can drag content from a note into Share. On reload, QuoteBlock shows live content. On publish, snapshot is frozen.

## Phase 7 — Track (Month 7–8, provisionally)

Reassess timing after Layer is working and in daily use.

- [ ] Implement JSON Canvas file format (read/write)
- [ ] Build tldraw ↔ JSON Canvas adapter
- [ ] Build canvas with note-reference nodes (compact state)
- [ ] Build expanded live preview state (drag corner → snap)
- [ ] Build Linear layout mode (sequence axis)
- [ ] Build Spatial layout mode (free-position)
- [ ] Build connection tool (exactly two nodes, visual-only)
- [ ] Build annotation nodes (anchor to node or connection)
- [ ] Build "Add new item" modal (search vault, add as reference node)
- [ ] **Gate:** Can create a Track, add notes, connect them, annotate. Canvas saves as valid JSON Canvas format.

## Phase 8 — Desk (integrated throughout, finalised here)

Desk canvas is partially built during other phases. Finalise here.

- [ ] Build full Desk canvas with tldraw
- [ ] Freeform nodes (ephemeral text)
- [ ] Note-reference nodes (link to existing note)
- [ ] Canvas-reference nodes (link to Track canvas, render live preview)
- [ ] "Capture to Inbox" (convert freeform node in-place)
- [ ] Timer pill (wipe, history, frequency)
- [ ] History mode (read-only, amber border)
- [ ] **Gate:** All three node types work. Timer wipes and archives correctly. History mode is read-only.

## Phase 9 — Polish + launch (Months 8–9)

- [ ] Keyboard shortcuts throughout
- [ ] Error states (deleted source note, missing reference, corrupt file)
- [ ] Performance: large vaults (1000+ notes)
- [ ] Onboarding (empty state, first capture)
- [ ] Payments integration (Paddle or Lemon Squeezy)
- [ ] App icon, notarisation, Sparkle updates
