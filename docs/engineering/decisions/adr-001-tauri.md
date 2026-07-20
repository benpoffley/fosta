---
title: "ADR-001: Tauri 2"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 1
layout: default
---

# ADR-001: Tauri 2 as the desktop shell

**Status:** Final  
**Date:** June 2026  
**Last reviewed:** July 2026

## Decision

Tauri 2 is the desktop application shell for Fosta v1. It wraps a React/TypeScript frontend in a native macOS window using the system WebView (WKWebView).

---

## Context

Fosta is designed as a macOS-native application. The creator has strong web skills (React, TypeScript) but no Swift/SwiftUI experience. A hybrid approach allows a web-based UI with native desktop capabilities — local filesystem access, SQLite, native window management — without requiring a full native language rewrite.

The core architecture depends on native OS access: markdown files live on disk, SQLite runs locally, the StorageAdapter reads and writes files directly. These requirements ruled out a pure web app for v1.

---

## Why Tauri over the alternatives

| Option | Decision | Reason |
|---|---|---|
| **Tauri 2** | ✅ Chosen | ~3MB binary, uses system WKWebView (not bundled Chromium), native filesystem and SQLite plugins, entire UI in React/TypeScript |
| **Electron** | ❌ Rejected | ~150MB binary, bundles Chromium, high memory usage, does not feel native |
| **SwiftUI native** | ❌ Rejected | Requires learning Swift from scratch; Tiptap (the block editor) has no SwiftUI equivalent; would eliminate AI-assisted development via Cursor |
| **Web app** | ❌ Rejected for v1 | Local file access and SQLite require native integration; File System Access API is inconsistent across browsers and unavailable on iOS Safari |
| **React Native (macOS)** | ❌ Rejected | macOS target is immature; complex desktop interactions (canvases, drag, three-panel layout) are poorly supported |

---

## What Tauri gives us

- **Lightweight:** ~3MB binary, uses macOS's built-in WebView — no bundled browser engine
- **Native feel:** system fonts, native menus, native window chrome, proper dock icon
- **Filesystem access:** first-class via Tauri's `fs` plugin — reads and writes markdown files directly
- **SQLite:** via Tauri SQL plugin — runs locally, zero latency, rebuildable from markdown files
- **Developer experience:** entire UI written in React/TypeScript, compatible with Cursor AI-assisted development
- **Rust backend:** performant, safe systems code for OS integration without requiring the creator to write it

---

## What Tauri does not give us

- **iOS or Android support:** Tauri is desktop-only. Mobile requires a separate build.
- **Browser access:** a Tauri app cannot be opened in a web browser.
- **Automatic cross-device sync:** handled separately via Cloudflare (see ADR-010).

These limitations are accepted for v1. The mobile and cross-platform strategy is documented separately in ADR-013.

---

## Why not to revisit

The entire build strategy — plugin architecture, filesystem access, SQLite integration, StorageAdapter implementation — is built around Tauri. Changing the shell would mean rebuilding the application from scratch. This decision is final for v1. The question of whether Tauri remains the right shell for v2 and beyond should be revisited once v1 is shipping and mobile requirements are clearer.
