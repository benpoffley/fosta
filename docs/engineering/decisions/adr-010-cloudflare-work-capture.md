---
title: "ADR-010: Cloudflare work capture"
parent: "Decision Records"
nav_order: 10
layout: default
---

# ADR-010: Cloudflare work-capture architecture

**Status:** Final for v1  
**Date:** June 2026

## Decision
Browser form → Cloudflare Worker → D1 → home Mac polls and pulls → markdown files into `Inbox/`. Home Mac pushes read-only static snapshot to R2. No editing at work. One-way flows.

## Why this architecture
- Solves capture at work and read access without a full sync engine
- One-way flows eliminate sync conflicts entirely
- No editing at work is an accepted constraint that simplifies the architecture enormously
- Cloudflare Workers + D1 + R2 is inexpensive for this use case

## Known limitations
- Polling latency between capturing at work and notes appearing on Mac
- Read-only vault at work — accepted for v1
- Live transclusion does not work across Cloudflare — work vault displays `snapshot_text` only

## Future
v2 may introduce a proper sync engine via the StorageAdapter pattern. This ADR documents the deliberate v1 simplification.
