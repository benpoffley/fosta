# ADR-001: Tauri 2 as the desktop shell

**Status:** Final  
**Date:** June 2026

## Decision
Tauri 2 is the desktop application shell. It wraps a React/TypeScript frontend in a native macOS window using the system WebView (WKWebView).

## Context
Fosta is a macOS-native application. The creator has strong web skills (React, TypeScript) but no Swift/SwiftUI experience. A hybrid approach allows web-based UI with native desktop capabilities.

## Why Tauri
- ~3MB binary vs Electron's ~150MB — feels lightweight and native
- Uses system WebView, not bundled Chromium — lower memory, better OS integration
- Strong Rust backend with first-class filesystem and SQLite via official plugins
- Active v2 development with better plugin architecture than v1
- Entire UI in React/TypeScript — the creator's existing skill set

## Alternatives considered
- **Electron:** Rejected. Large binary, high memory, does not feel native.
- **SwiftUI native:** Rejected. Requires learning Swift from scratch. Tiptap has no SwiftUI equivalent.
- **Web app:** Rejected for v1. Local file access and SQLite require native integration.

## Why not to revisit
The entire build strategy, plugin architecture, filesystem access, and SQLite integration depend on Tauri. Changing the shell means rebuilding the application.
