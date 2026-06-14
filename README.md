# MEXC Live Stats — backend service

Private **FastAPI + PostgreSQL + Node SSR** pipeline that ingests MEXC spot trades over protobuf WebSockets, aggregates pair stats, fans out live data, and feeds SEO snapshots to WordPress.

**Private code:** [logicencoder/mexc-live-stats-backend](https://github.com/logicencoder/mexc-live-stats-backend)

**Product overview (portfolio):** [mexc-live-stats-plugin-overview](https://github.com/logicencoder/mexc-live-stats-plugin-overview) — live dashboard at [logicencoder.com/mexc-app/](https://logicencoder.com/mexc-app/)

## Service role

- **`mexc_td_app.py`** — MEXC V3 WS ingest, asyncpg batch writes, REST + `/ws` (msgpack), operator `/dashboard`
- **`mexc_td_app_ssr.js`** — React SSR for per-coin SEO pages (`/ssr_m1/{symbol}`)
- PostgreSQL `trades` table, stats cache, snapshot POST to WordPress `mexc/v1/snapshot`
- Public tunnel: `wss://ws.logicencoder.com/ws`

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
