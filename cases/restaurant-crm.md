# Case: Restaurant CRM

Operational management system for delivery networks.

---

## Problem

Delivery restaurants operate without real visibility into the business. Orders are tracked across fragmented platforms, margins aren't calculated per product, customer behavior isn't analyzed, and operational decisions are made by guesswork. The larger the network, the bigger the information black hole.

---

## Solution

Complete operational CRM with:

- **Real-time dashboards** — orders, revenue, average ticket, margins per product and per store
- **Order tracking** — order status from receipt to delivery, with timestamps at each stage
- **Customer management** — order history, frequency, total spend, automatic segmentation
- **Gamification** — loyalty system with levels, points, and rewards based on real behavior
- **Multi-store** — each unit sees its own data, central management sees everything aggregated
- **Reports** — data export, period comparisons, anomaly alerts

---

## Architecture

```
Frontend: Next.js 14 (App Router) + TypeScript + Tailwind CSS
ORM: Drizzle ORM (type-safe, explicit SQL)
Database: PostgreSQL via Neon (serverless, branching for previews)
Auth: NextAuth.js with RBAC per organization
Deploy: Vercel (preview per PR, production on main)
Monitoring: Sentry (error tracking) + Lighthouse CI (performance)
CI: GitHub Actions (lint + typecheck + tests + build + deploy)
```

### Architecture decisions

- **Drizzle over Prisma:** explicit queries, no binary engine, better debugging. In a system with complex aggregation queries (margins, metrics), knowing exactly which SQL runs is critical.
- **Neon over Supabase:** native branching allows each PR to have its own test database. No conflict between environments.
- **NextAuth with custom RBAC:** each organization (restaurant) is isolated. Network admin sees all units. Store manager sees only theirs.
- **Server Components by default:** data loads on the server, less JavaScript on the client, better performance on mobile devices (operators frequently use their phones).

---

## Results

- **352 tests** — unit, integration, and E2E
- **CI always green** — complete pipeline runs in under 4 minutes
- **Sentry integrated** — production errors tracked with full context
- **Lighthouse CI** — score above 90 in performance, accessibility, and SEO
- **Zero downtime** — atomic deploys via Vercel with automatic rollback
- **Functional multi-tenancy** — data isolated per organization, tested with multiple units simultaneously

---

## Lessons learned

1. **Gamification looks simple, is complex.** Scoring rules, levels, rewards, and edge cases (canceled order that already gave points, point expiration) require careful modeling. Don't underestimate it.

2. **Dashboards are the most political feature.** Every stakeholder wants different metrics. Defining the indicators before building the dashboard avoids infinite rework.

3. **Multi-tenant RBAC needs extensive testing.** The question "can user X see data from organization Y?" needs to be answered by automated tests, not manual review.

4. **Drizzle + Neon is an efficient combination.** Migrations as code, branching per PR, end-to-end type safety. The development cycle became significantly faster.

5. **Lighthouse CI in the pipeline catches performance regressions before deploy.** Bundle grew? Unoptimized image? CI warns before it hits production.
