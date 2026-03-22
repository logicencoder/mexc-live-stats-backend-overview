# MEXC Exchange - Live Trading Statistics Backend (Overview)

Public overview repository for the private FastAPI backend implementation.

> Source code is private by design. This repo documents architecture, stack, and delivered capabilities without exposing proprietary code.

## Live status

- Product mode: live backend service + WordPress plugin frontend integration
- Live URL (frontend): `https://logicencoder.com/mexc-app/`
- Backend origin shown here as architecture only (private runtime)

## Problem solved

A single backend that can ingest high-frequency MEXC trade streams, persist data efficiently, compute usable analytics in near real time, and publish SEO snapshots that can be indexed by search engines.

## Tech stack

- Python
- FastAPI
- AsyncIO
- asyncpg (PostgreSQL connection pool)
- aiohttp (snapshot upload HTTP client)
- orjson / msgpack
- Pillow and/or matplotlib (chart images)
- Uvicorn

## Architecture (high-level)

1. MEXC WebSocket trade ingestion
2. Async buffering + batched PostgreSQL writes
3. Rolling stats cache updates by symbol
4. Real-time delivery through API/WebSocket
5. Periodic snapshot generation (HTML + JSON-LD + chart image)
6. Snapshot push to WordPress REST API

## Key API surface (representative)

- `GET /api/stats`
- `GET /api/monitoring`
- `GET /api/symbols`
- `GET /api/stats/memory/{symbol}`
- `GET /api/trades`
- `WS /ws`

## Snapshot/SEO pipeline

- Generates symbol-focused snapshot pages
- Includes structured data (Schema.org JSON-LD)
- Supports chart image generation for richer pages
- Sends payloads to WordPress endpoint:
  - `POST /wp-json/mexc/v1/snapshot`

## Security and privacy boundary

- Private repository contains implementation details and deployment specifics
- Public repo intentionally excludes:
  - API keys
  - server credentials
  - internal infrastructure config
  - private optimization internals

## Operational focus

- Async-first runtime for throughput
- Database pooling and write batching
- Bounded update loops + periodic cleanup
- Practical performance tuning for real production load

## Screenshots

![MEXC Live Trading Statistics Dashboard](./screenshot.png)

## Author note

Built and iterated as a self-taught, research-first engineering workflow: identify bottlenecks, measure behavior, optimize for reliability, then expose only the parts safe for public portfolio review.
