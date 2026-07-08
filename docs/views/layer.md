# Layer / Work / Develop

**Status:** Decided  
**Name:** TBD — candidates: Layer, Work, Develop  
**Note:** "Layer" fails the CTA test — "Open in Layer" reads oddly.

## What it is

The main editor view. Deep work — block-typed editing, backlinks, comments, metadata. Where ideas are developed into real content.

## Layout — three panels

| Panel | Contents | Collapsible? |
|---|---|---|
| Left | File navigator — Obsidian-style vault browser | Yes |
| Centre | Block-typed Tiptap editor with tab bar | No |
| Right | Layers panel — metadata + backlinks (top), block comments + tags (bottom, when block selected) | Yes |

## File navigator (left panel)

Obsidian-style vault browser. Full folder operations (create, rename, move, delete). Same data as Sort — different activity. Single-click opens a note directly in the editor. Double-click does nothing.

## Editor (centre panel)

Tiptap block-typed editor. Features:
- Slash `/` command opens block type selector
- Tab bar for multiple open notes
- Every block has a stable UUID (never regenerated)
- Block UUIDs serialised as HTML comments: `<!-- id: 550e8400 -->`

## Layers panel (right panel)

**When no block selected:**
- Note metadata (title, tags, created, modified)
- Backlinks — notes that link to this note via wikilinks

**When a block is selected:**
- Block-level comments (notes with `type: comment`, `parentBlock: <uuid>`)
- Block-level tags

## Block-level comments

Comments are notes with this frontmatter:
```yaml
type: comment
parentNote: <note-uuid>
parentBlock: <block-uuid>
```

Anchored to the block UUID. If the block is moved within the note, the comment follows. If the block is deleted, the comment is orphaned (warning shown).

## Wikilinks

Syntax: `[[display-name|uuid]]`

Clicking a wikilink opens the linked note in Quick Look modal (not directly in the editor). This lets the user reference without losing their place. Back arrow (←) in Quick Look navigates the stack.

## Split view

Deferred to v1.1.

## Add to Desk

"Add to Desk" is available as a direct button in the Layer editor (not buried in overflow). See `../foundations/global-actions.md`.
