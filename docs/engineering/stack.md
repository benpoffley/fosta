# Stack

**Status:** Final — do not suggest alternatives

## The stack

| Layer | Choice | Notes |
|---|---|---|
| Desktop shell | Tauri 2 | Not Electron. See ADR-001. |
| UI framework | React 18 + TypeScript | Always TypeScript, never plain JS |
| Styling | Tailwind CSS | |
| Rich text editor | Tiptap | Block-typed, stable block UUIDs |
| Canvas renderer | tldraw | Renderer only — never storage |
| Canvas storage format | JSON Canvas | Open spec, not tldraw's internal format |
| Storage: source of truth | Markdown files on disk | YAML frontmatter, human-readable |
| Storage: index/query | SQLite via Tauri SQL plugin | Rebuildable index, never source of truth |
| State management | Zustand | |
| Backend (work access only) | Cloudflare Workers + R2 + D1 | Not in critical path |
| Payments | Paddle or Lemon Squeezy | TBD |
| IDE | Cursor | AI-assisted development |
| Version control | Git + GitHub | |

## Key resources

- tauri.app
- tiptap.dev
- tldraw.dev
- developers.cloudflare.com/workers
- github.com/pmndrs/zustand
- react.dev
- tailwindcss.com/docs
