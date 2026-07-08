# Architecture Principles

**Status:** Final — non-negotiable

These nine principles govern every engineering decision in Fosta. See `decisions/` for full ADRs with context and rationale.

## The nine principles

### 1. Storage adapter pattern (ADR-004)
All application code interacts with storage through a `StorageAdapter` interface. Never call filesystem APIs directly.

```typescript
interface StorageAdapter {
  listNotes(): Promise<NoteMetadata[]>
  getNote(id: string): Promise<Note>
  saveNote(id: string, content: string): Promise<void>
  deleteNote(id: string): Promise<void>
  watchForChanges(callback: (id: string) => void): Promise<void>
}
```

`LocalFilesAdapter` is the v1 implementation. v2 backends (Google Drive, iCloud) implement the same interface without touching application code.

### 2. UUIDs as stable identity (ADR-005)
Every note has a UUID in YAML frontmatter. All references use UUID. File paths are incidental.

```yaml
id: 550e8400-e29b-41d4-a716-446655440000
```

### 3. Everything is a note (ADR-008)
Comments, quotes, share documents — all are notes with different frontmatter. No parallel object types. Type is expressed as a `type` field: `comment`, `quote`, `share`. Standard notes have no type field.

### 4. References not copies (ADR-009)
Notes in Track, Share, or Desk are UUID references to originals. Never duplicate content. Deleted source → warning, not silent break.

### 5. Async everywhere
Every storage call is async/await. No exceptions.

### 6. Offline-first (ADR-007)
Core functionality works without internet. Cloudflare is work-access only — not in the critical path.

### 7. YAML frontmatter on every note (ADR-002)
Required fields: `id`, `created`, `modified`, `tags`. OS file dates are unreliable.

### 8. Block UUIDs are permanent (ADR-012)
Every Tiptap block gets a UUID at creation, serialised as an HTML comment. Never regenerated. Block comment anchors and transclusion anchors depend on this.

```markdown
This is a paragraph. <!-- id: 550e8400 -->
```

### 9. JSON Canvas for canvases (ADR-006, ADR-011)
Canvas files stored as JSON Canvas format. tldraw is the renderer only — its internal format never written to disk.

## Data flow

```
Markdown files (source of truth)
       ↓ scan on startup / file watch
    SQLite index (rebuildable)
       ↓ queries
  React UI (Zustand state)
       ↓ saves via StorageAdapter
Markdown files (source of truth)
```

## The StorageAdapter pattern in practice

```
Application code
      ↓
StorageAdapter interface
      ↓
LocalFilesAdapter (v1)     ←→     Markdown files on disk
                                        ↕
                                   SQLite index
```

Never shortcut through to the filesystem. The adapter is what makes v2 backends possible without touching application logic.
