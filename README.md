# MEXC Live Stats — backend service

Private **FastAPI + PostgreSQL + Node SSR** pipeline for [logicencoder.com/mexc-app/](https://logicencoder.com/mexc-app/). Ingests MEXC spot trades over protobuf WebSockets, persists history, aggregates pair statistics, fans out live data to browsers, and POSTs SEO snapshots to WordPress.

**Private code:** [logicencoder/mexc-live-stats-backend](https://github.com/logicencoder/mexc-live-stats-backend)

**Public product (portfolio):** [mexc-live-stats-plugin-overview](https://github.com/logicencoder/mexc-live-stats-plugin-overview)

The plugin renders the dashboard and static snapshot HTML. This backend is ingestion, storage, and realtime API.

## Data flow

```
MEXC spot WebSocket (protobuf V3)
        │
        ▼
mexc_td_app.py — parse → queue → asyncpg batch insert
        │
        ├── PostgreSQL `trades`
        ├── WebSocket /ws (msgpack) → plugin + visitors
        ├── REST stats → SSR + operator dashboard
        └── snapshot POST → wp-json/mexc/v1/snapshot
```

Public live stream: **`wss://ws.logicencoder.com/ws`**

## Python ingest (`mexc_td_app.py`)

- Connects to **`wss://wbs-api.mexc.com/ws`** with generated protobuf bindings in `generated_proto/`
- Sharded connections and precision cache (`cached_mexc_precision.json`, `mexc_usdt_pairs.py`)
- **`ServerConfig`** from environment: PostgreSQL credentials, port (default **8080**), CORS, snapshot URL/key
- Stats cache rebuilt for REST and WebSocket fan-out — not recomputed per client

## PostgreSQL and REST

Historical trades in **`trades`** table. Key REST endpoints:

| Path | Role |
|------|------|
| `/ws` | Live trade stream to browsers |
| `/ws-dashboard`, `/ws-dashboard-logs` | Operator monitoring streams |
| `/api/stats`, `/api/stats/memory/{symbol}` | Aggregated 24h and rolling pair stats |
| `/api/bootstrap/all`, `/api/trades` | Client bootstrap and history |
| `/api/coins`, `/api/reload-symbols`, `/api/dead-symbols` | Symbol list management |
| `/api/orderbook`, `/api/orderbook/live` | Orderbook snapshots |
| `/api/monitoring`, `/api/metrics`, `/api/storage-stats` | Health and ops |
| `/ssr_m1/{symbol}` | JSON bundle for Node SSR |

Outbound: **`POST https://logicencoder.com/wp-json/mexc/v1/snapshot`** with API key — writes static HTML under `/snapshots/` on WordPress for crawlers and social previews.

## Node SSR (`mexc_td_app_ssr.js`)

Express + React SSR (default port **3000**) fetches Python API and renders HTML + JSON-LD for **`/mexc/{SYMBOL}/`** SEO pages. Distinct from the snapshot pipeline — both feed discoverability.

## Operator dashboard

**`dashboard.html`** + **`dashboard.js`** — queue depth, subscription health, connection status for the same process (not a separate product).

## Distinction from other Logic Encoder repos

**`mexc_trading_app`** is a local self-hosted trading console with order placement — separate codebase. This backend only ingests **public** spot trades for the stats site.

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
