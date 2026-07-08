---
title: "Acceptance Criteria"
parent: "Build"
nav_order: 4
layout: default
---

# Acceptance Criteria

Definition of "done" for each major feature. A feature is not done until all criteria pass.

## General criteria (apply to everything)

- [ ] Feature works correctly on macOS Sonoma and Sequoia
- [ ] No TypeScript errors
- [ ] All note operations go through StorageAdapter (no direct filesystem calls)
- [ ] Data written is valid Markdown + YAML frontmatter
- [ ] Feature works correctly after app restart (data persists)
- [ ] Error states are handled (missing file, corrupt frontmatter, etc.)

## Capture

- [ ] Typing in capture bar creates a `.md` file in `Inbox/` on save
- [ ] Created note has `id`, `created`, `modified`, `tags: []` in frontmatter
- [ ] UUID in frontmatter is a valid UUID v4
- [ ] Capture bar is always visible on Desk
- [ ] Capture bar on other views: hidden until + clicked, collapses after idle, stays open once typing
- [ ] "Open in Layer" opens the note in the Layer editor
- [ ] Discarding a capture does not create a file

## StorageAdapter / LocalFilesAdapter

- [ ] `listNotes()` returns all `.md` files in the vault
- [ ] `getNote(id)` returns the correct note by UUID (not file path)
- [ ] `saveNote(id, content)` writes correct Markdown + YAML
- [ ] `deleteNote(id)` removes the file
- [ ] `watchForChanges()` fires when a file is modified externally
- [ ] SQLite index is rebuilt correctly if the `.fosta/fosta.db` file is deleted

## Block UUIDs (critical)

- [ ] Every new block in Tiptap has a UUID as a node attribute
- [ ] Block UUIDs are serialised as `<!-- id: uuid -->` in Markdown
- [ ] Block UUIDs are restored from HTML comments on load
- [ ] Block UUIDs do not change through a save → reload cycle
- [ ] Block UUIDs do not change when content above or below the block is edited
- [ ] Confirm with explicit test: create note, add 3 blocks, record UUIDs, save, reload, confirm UUIDs unchanged

## Quick Look

- [ ] Single click on canvas note-reference node selects it
- [ ] Double click on canvas note-reference node opens Quick Look (note variant)
- [ ] Quick Look note variant: shows title and body, no right panel
- [ ] Quick Look autosaves on every keystroke via `saveNote()`
- [ ] "Add to Desk" creates a note-reference node on the Desk scratchpad
- [ ] "Open in Layer" opens the note in the Layer editor and closes Quick Look
- [ ] Clicking a wikilink inside Quick Look opens the linked note in the same modal
- [ ] Back arrow appears after navigating via wikilink
- [ ] Back arrow returns to the previous note
- [ ] Navigation stack is empty when Quick Look is reopened fresh
- [ ] Quick Look canvas variant is read-only (no edits possible)

## Canvas (JSON Canvas)

- [ ] Desk scratchpad saves as valid JSON Canvas format
- [ ] Track canvases save as valid JSON Canvas format
- [ ] JSON Canvas files are human-readable JSON
- [ ] tldraw's internal format is never written to disk
- [ ] Canvas round-trips correctly: create → save → reload → same nodes in same positions
- [ ] Compact ↔ expanded state transition works at correct size threshold
- [ ] Expanded note nodes show live note content
- [ ] Expanded canvas-reference nodes show read-only Track canvas via tldraw

## Share + transclusion

- [ ] QuoteBlock resolves content from SQLite at render time
- [ ] If source note is edited, QuoteBlock reflects the change on next render
- [ ] `snapshot_text` is saved at QuoteBlock creation time
- [ ] On publish, all QuoteBlocks freeze to `snapshot_text`
- [ ] `published: true` written to Share note frontmatter on publish
- [ ] Drag-to-quote creates a QuoteBlock with correct source UUID and block references
- [ ] Smart Paste creates a QuoteBlock (not a plain text copy)
