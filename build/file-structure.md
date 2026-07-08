# Target File Structure

The intended folder structure for the Fosta application codebase.

```
fosta-app/                          ← Tauri project root
├── src-tauri/                      ← Rust/Tauri backend
│   ├── src/
│   │   ├── main.rs
│   │   ├── commands/               ← Tauri commands exposed to frontend
│   │   │   ├── notes.rs            ← CRUD operations
│   │   │   ├── vault.rs            ← vault scanning, file watching
│   │   │   └── canvas.rs           ← JSON Canvas file ops
│   │   └── db/
│   │       └── schema.sql          ← SQLite schema
│   └── Cargo.toml
│
├── src/                            ← React/TypeScript frontend
│   ├── main.tsx
│   ├── App.tsx                     ← root, toolbar, view router
│   │
│   ├── types/                      ← TypeScript interfaces
│   │   ├── note.ts                 ← Note, NoteMetadata, NoteType
│   │   ├── canvas.ts               ← CanvasNode, CanvasEdge, NodeState
│   │   └── storage.ts              ← StorageAdapter interface
│   │
│   ├── storage/                    ← Storage layer
│   │   ├── StorageAdapter.ts       ← interface definition
│   │   ├── LocalFilesAdapter.ts    ← v1 implementation
│   │   └── index.ts                ← export active adapter
│   │
│   ├── db/                         ← SQLite index layer
│   │   ├── index.ts                ← db connection
│   │   ├── scanner.ts              ← vault scanner
│   │   ├── queries/
│   │   │   ├── notes.ts
│   │   │   ├── tags.ts
│   │   │   ├── backlinks.ts
│   │   │   └── transclusions.ts
│   │
│   ├── store/                      ← Zustand state
│   │   ├── vaultStore.ts           ← vault state, active note
│   │   ├── uiStore.ts              ← panel states, modal state
│   │   └── captureStore.ts         ← capture bar state
│   │
│   ├── components/
│   │   ├── shared/
│   │   │   ├── Toolbar.tsx         ← bottom nav bar
│   │   │   ├── CaptureBar.tsx      ← global capture bar
│   │   │   ├── QuickLook.tsx       ← Quick Look modal wrapper
│   │   │   ├── QuickLookNote.tsx   ← note variant
│   │   │   ├── QuickLookCanvas.tsx ← canvas variant
│   │   │   ├── FileNavigator.tsx   ← shared across Layer + Share
│   │   │   └── TagBubble.tsx       ← tag with hover/Create Track
│   │   │
│   │   ├── desk/
│   │   │   ├── DeskView.tsx
│   │   │   ├── CanvasNodeCompact.tsx
│   │   │   ├── CanvasNodeExpanded.tsx
│   │   │   └── TimerPill.tsx
│   │   │
│   │   ├── sort/
│   │   │   ├── SortView.tsx
│   │   │   ├── NoteCard.tsx
│   │   │   └── InboxGrid.tsx
│   │   │
│   │   ├── layer/
│   │   │   ├── LayerView.tsx
│   │   │   ├── Editor.tsx          ← Tiptap wrapper
│   │   │   ├── TabBar.tsx
│   │   │   └── LayersPanel.tsx
│   │   │
│   │   ├── track/
│   │   │   ├── TrackView.tsx
│   │   │   ├── TrackCanvas.tsx     ← tldraw wrapper
│   │   │   └── AddItemModal.tsx
│   │   │
│   │   └── share/
│   │       ├── ShareView.tsx
│   │       ├── PreviewPane.tsx
│   │       ├── ShareNote.tsx
│   │       └── QuoteBlock.tsx
│   │
│   ├── canvas/
│   │   └── JsonCanvasAdapter.ts    ← tldraw ↔ JSON Canvas translation
│   │
│   ├── editor/
│   │   ├── extensions/
│   │   │   └── BlockUUID.ts        ← Tiptap extension for block UUIDs
│   │   ├── serialiser.ts           ← Tiptap → Markdown
│   │   └── deserialiser.ts         ← Markdown → Tiptap
│   │
│   └── lib/
│       ├── uuid.ts                 ← UUID generation
│       ├── frontmatter.ts          ← YAML frontmatter parse/serialise
│       └── wikilinks.ts            ← wikilink parsing
│
└── package.json
```

## User vault structure (on disk)

```
~/Documents/Fosta/                  ← or user-chosen location
├── Inbox/                          ← all captures land here
├── Projects/                       ← user-organised folders
├── Archive/                        ← user-archived notes
├── Scratchpad/
│   └── Archive/                    ← timed Desk archives
│       └── 2026-04-24-1430.canvas
├── .fosta/
│   └── fosta.db                    ← SQLite index (rebuildable)
└── *.md                            ← notes (also in subfolders)
```
