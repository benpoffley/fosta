# Fosta

> The non-linear editor for thought. A studio for your ideas.

Fosta is a macOS-native note-taking and idea-development app built around a pipeline metaphor. Notes flow through five views switchable via a persistent bottom toolbar.

**Domain:** fosta.studio  
**Status:** Month 1 — Pre-code foundation  
**Stack:** Tauri 2 · React 18 + TypeScript · Tailwind · Tiptap · tldraw · SQLite · Cloudflare

---

## This repository

This repo contains the Fosta product wiki — the single canonical source of truth for the product. It serves two purposes:

1. **Product constitution** — defines what Fosta is, why decisions were made, and what the constraints are
2. **Build reference** — structured for AI coding tools (Claude Code, Cursor) to use as context when implementing features

## Navigation

| Section | Purpose |
|---|---|
| [`docs/product/`](docs/product/) | Vision, philosophy, positioning, views overview |
| [`docs/engineering/`](docs/engineering/) | Stack, architecture, data model, ADRs, guardrails |
| [`docs/views/`](docs/views/) | Full spec for each view |
| [`docs/foundations/`](docs/foundations/) | Capture, Global Actions, Quick Look |
| [`docs/project/`](docs/project/) | Current status, roadmap, story, changelog |
| [`build/`](build/) | Build guide, file structure, sequencing, acceptance criteria |

## For AI coding tools

**Start with [`CLAUDE.md`](CLAUDE.md)** — it contains the guardrails and a map of the codebase. Read it before writing any code.

## Order of authority

When sections conflict, higher beats lower:

1. Architectural Decision Records (ADRs) — `docs/engineering/decisions/`
2. AI Guardrails — `CLAUDE.md` and `docs/engineering/ai-guardrails.md`
3. Product philosophy + architecture — `docs/product/` and `docs/engineering/`
4. Feature specifications — `docs/views/` and `docs/foundations/`
5. Current development — `docs/project/current-development.md`
6. Roadmap — lowest authority
