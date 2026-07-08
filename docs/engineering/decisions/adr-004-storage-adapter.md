---
title: "ADR-004: Storage adapter"
parent: "Decision Records"
nav_order: 4
layout: default
---

# ADR-004: Storage adapter pattern

**Status:** Final  
**Date:** June 2026

## Decision
All application code interacts with storage through a `StorageAdapter` interface. `LocalFilesAdapter` is the v1 implementation. Application code never calls filesystem APIs directly.

## Interface
```typescript
interface StorageAdapter {
  listNotes(): Promise<NoteMetadata[]>
  getNote(id: string): Promise<Note>
  saveNote(id: string, content: string): Promise<void>
  deleteNote(id: string): Promise<void>
  watchForChanges(callback: (id: string) => void): Promise<void>
}
```

## Context
v1 stores notes as local Markdown files. v2 will need alternative backends (Google Drive, iCloud). Without an adapter pattern, adding a new backend requires modifying application code throughout the codebase.

## Why the adapter pattern
- v2 backends can be added by implementing the interface without touching application logic
- Testable — swap adapter for a mock in tests
- Enforces separation between "how notes are stored" and "what the app does with notes"

## Why not to revisit
Bypassing the adapter for "simplicity" creates technical debt that becomes expensive when v2 backends are introduced.
