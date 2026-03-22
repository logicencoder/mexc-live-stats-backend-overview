# MEXC Exchange - Live Trading Statistics Backend (Overview)

Public overview repository for the private FastAPI backend implementation.

> Source code is private by design. This repo documents architecture, stack, and delivered capabilities without exposing proprietary code.

## Positioning

This backend is the live processing engine behind the public dashboard at `https://logicencoder.com/mexc-app/`.
It ingests high-frequency MEXC trades, computes rolling analytics, and publishes SEO-ready snapshots to WordPress.

- Current delivery mode: live FastAPI backend + live WordPress plugin frontend
- Live frontend URL: `https://logicencoder.com/mexc-app/`
- Source exposure mode: architecture and capability overview only

## UI Snapshot

![MEXC Live Trading Statistics Dashboard](./screenshot.png)

## Product Summary

The backend solves one practical problem: keep a real-time trading statistics surface responsive while also generating searchable, indexable snapshot content for long-tail discovery.

## Tech Stack Used

- **Backend runtime**: Python, FastAPI, AsyncIO, Uvicorn
- **Realtime/data path**: WebSocket ingestion, async buffering, rolling in-memory stats
- **Database**: PostgreSQL via `asyncpg` connection pooling
- **Serialization/transport**: `orjson`, `msgpack`, `aiohttp`
- **Snapshot rendering**: HTML + Schema.org JSON-LD + chart image generation (`Pillow` / `matplotlib`)
- **Integration target**: WordPress REST endpoint (`POST /wp-json/mexc/v1/snapshot`)

## High-Level Architecture

- **Ingestion Layer**: subscribes to MEXC trade streams and normalizes event flow
- **Processing Layer**: maintains bounded rolling metrics and symbol-specific aggregates
- **Persistence Layer**: performs batched async writes to PostgreSQL
- **Delivery Layer**: serves API/WebSocket updates for frontend consumption
- **Snapshot Layer**: generates publishable pages with structured data and chart assets
- **Integration Layer**: pushes snapshots to WordPress plugin REST endpoint

## Representative API Surface

- `GET /api/stats`
- `GET /api/monitoring`
- `GET /api/symbols`
- `GET /api/stats/memory/{symbol}`
- `GET /api/trades`
- `WS /ws`

## Core Capability Inventory

### A) Realtime Ingestion and Processing

1. High-frequency MEXC stream ingestion
2. Async trade buffering and queue control
3. Rolling symbol-level metric updates
4. In-memory fast-read cache for frontend requests
5. WebSocket + REST parallel delivery support

### B) Snapshot and SEO Publishing

6. Symbol-focused snapshot page generation
7. Schema.org JSON-LD structured payload creation
8. Chart image rendering for snapshot pages
9. WordPress snapshot POST integration (`/wp-json/mexc/v1/snapshot`)
10. Periodic regeneration workflow for fresh indexed pages

### C) Reliability and Operations

11. Async PostgreSQL pooling and batched writes
12. Bounded update loops with periodic cleanup
13. Runtime monitoring endpoints and diagnostics
14. Environment-variable based secret/runtime configuration (`.env` compatible)
15. Production-oriented throughput optimization for constrained servers

## Who This Overview Is For

### Recruiters

Use this repo as a concise map of practical backend ownership: realtime ingestion, async processing, persistence, and integration delivery.

### System Engineers

Use this repo to evaluate ingestion-to-delivery pipeline design, cache/write tradeoffs, and snapshot publishing architecture.

### Collaborators

Use this repo to understand where new analytics modules, symbols, or publishing targets can be extended.

### Potential Employers

Use this repo as evidence of production-minded backend execution: fast event handling, stable data flow, and measurable optimization.

## Security & Disclosure Policy

- Private repositories retain implementation internals and deployment specifics.
- Public overview intentionally excludes API keys, server credentials, and private infrastructure wiring.
- This repository is documentation-first and non-sensitive by design.

## Related Private Repositories

- `logicencoder/mexc-live-stats-backend-private`
- `logicencoder/mexc-live-stats-plugin-private` (integration counterpart)

## Author note

Built with a practical, self-taught workflow: identify bottlenecks, measure behavior, optimize reliability, and keep public documentation honest and non-hype.
