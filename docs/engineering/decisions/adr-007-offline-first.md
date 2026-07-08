# ADR-007: Offline-first architecture

**Status:** Final  
**Date:** June 2026

## Decision
Fosta works fully without internet. The Cloudflare backend serves work-capture only and is not in the critical path for any core functionality.

## Why offline-first
- User data always available regardless of connectivity
- No dependence on server uptime for core functionality
- Performance — local operations are always faster than network operations
- Consistent with the local-first, files-on-disk philosophy

## The Cloudflare exception
Work-capture (browser form → D1 → home Mac) and the read-only vault snapshot (R2) require Cloudflare. If unavailable, users cannot capture at work — but the core Mac app functions completely normally.

## Why not to revisit
Making core functionality network-dependent would require a full architectural rethink and break the local-first data ownership story.
