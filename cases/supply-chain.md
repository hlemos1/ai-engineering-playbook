# Case: Supply Chain China-Brazil

Import operations management system for China-Brazil trade.

---

## Problem

China-to-Brazil import operations are managed in disconnected spreadsheets. Each shipment involves dozens of documents, multiple suppliers, different timelines, and compliance requirements that change by product category. There's no unified view of each operation's status, and the risk of customs documentation errors is high — with real financial consequences.

---

## Solution

Unified tracking system with:

- **Shipment management** — each import as a unique entity with timeline, documents, costs, and status
- **Compliance scoring** — automatic score per shipment based on documentation completeness, supplier history, and product category
- **Operations dashboard** — consolidated view of all active, delayed, and completed shipments
- **Supplier management** — registry with performance history, real lead times, and reliability
- **Alerts** — automatic notification for pending documents, approaching deadlines, and cost anomalies
- **Cost calculation** — total landed cost (product + freight + taxes + storage) per shipment

---

## Architecture

```
Frontend: React + TypeScript + Tailwind CSS
Backend: Hono (API framework)
Runtime: Cloudflare Workers (edge deployment)
Database: Cloudflare D1 (SQLite at the edge)
Deploy: Cloudflare (wrangler)
CI: GitHub Actions (lint + tests + deploy)
```

### Architecture decisions

- **Cloudflare Workers + D1 over Node.js + PostgreSQL:** lightweight operation, few simultaneous users (import team), no need for a heavy relational database. D1 at the edge offers minimal latency for a team distributed between Brazil and China (different time zones, access from both sides).
- **Hono over Express:** framework built for the edge. End-to-end typing, composable middleware, natively compatible with Workers.
- **No heavy ORM:** D1 uses direct SQL with manual typing. For a simple schema and predictable queries, an ORM adds overhead without proportional benefit.
- **SPA over SSR:** internal dashboard, SEO irrelevant, users always authenticated. SPA simplifies deploy and reduces server dependency.

---

## Results

- **89 tests** — unit and integration
- **Global edge deployment** — latency < 50ms from both Brazil and China
- **Functional compliance scoring** — score calculated automatically based on 12 documented criteria
- **Near-zero infrastructure cost** — Workers free tier covers the team's usage volume
- **Deploy time: < 30 seconds** — push to main triggers automatic deploy via wrangler

---

## Lessons learned

1. **Edge-first works for internal tools.** Small teams with distributed users benefit from uniform global latency. You don't need a large system to justify edge.

2. **Compliance scoring needs explicit rules.** Each criterion must be documented, weighted, and auditable. An opaque score doesn't generate trust — the team needs to understand why a shipment has a low score.

3. **D1 has limitations.** For complex aggregation queries and multiple joins, SQLite at the edge shows its limits. For this use case (CRUD + scoring + alerts), it's sufficient. For heavy analytics, I'd migrate to PostgreSQL.

4. **Hono + Workers is the most productive combination for lightweight APIs.** Instant deploy, no cold start, complete typing. For microservices and internal tools, hard to justify anything heavier.

5. **Import operations are seasonal.** The system needs to handle periods of zero shipments and periods of 20+ simultaneous ones without degradation. Workers' automatic scaling solves this without configuration.
