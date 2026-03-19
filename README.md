# AI Engineering Playbook

**How to build real systems using AI as an execution layer. An operational manual.**

Most teams fail with AI not because the models are bad — but because there's no architecture underneath. AI without structure just generates mess faster.

This playbook is the model I use to go from business problem to production system consistently. It's extracted from building real products — not theory.

---

## Who built this

I'm the co-founder of a 200+ unit restaurant network (20+ brands, ~$45M/year revenue) where AI powers operations end-to-end: from supply chain management across China-Brazil import operations, to real-time delivery analytics, to sports analytics platforms with multi-agent AI systems.

Every pattern in this playbook comes from production systems with real users, real data, and real consequences when things break.

---

## Who this is for

- Engineers using AI to build products (not demos)
- Technical founders who code and ship
- Teams adopting AI-assisted development and hitting walls
- Anyone tired of "vibe coding" that doesn't survive production

## Who this is NOT for

- Looking for prompt collections or ChatGPT tricks
- Want AI to replace engineering (it amplifies it)
- Building demos that will never see real users

---

## Core thesis

> AI doesn't replace engineering. AI amplifies engineering — if there's architecture.

The human decides what to build, how to structure it, and which trade-offs to accept.
The AI generates code, tests, refactors, and documentation under human direction.
Together they produce more than either alone — but only if the human is in control.

---

## Contents

### Foundations
- **[Core Principles](principles/01-foundations.md)** — 6 rules that guide every technical decision
- **[Recommended Stack](stack/recommended-stack.md)** — Every choice has a reason. Nothing is here for hype
- **[Build Workflow](workflow/workflow.md)** — 7-step process from business problem to production

### Practice
- **[Real Prompts](prompts/real-prompts.md)** — Actual prompt patterns extracted from dev sessions (not theory)
- **[Common Mistakes](common-mistakes/mistakes.md)** — Failure patterns observed in practice, with corrections

### Case Studies (real systems, real numbers)
- **[Restaurant CRM](cases/restaurant-crm.md)** — Operational CRM for delivery networks. 352 tests. Multi-tenant RBAC. Gamification engine
- **[Supply Chain China-Brazil](cases/supply-chain.md)** — Import operations management. Edge-deployed globally. Compliance scoring. Near-zero infra cost
- **[Sports Analytics with AI](cases/cortex-fc.md)** — Football analytics platform. 7 specialized AI agents. 208 tests. Offline-first PWA

### Growth
- **[Maturity Roadmap](roadmap/roadmap.md)** — 4 levels from CRUD to interconnected AI-powered ecosystem

---

## The operating model

```
AI generates -> human validates -> AI adjusts -> production
```

This loop repeats at every stage. The AI never operates alone. The human never manually implements what AI can generate faster. The key: the human stays in control.

---

## The golden rule

**If you can't explain the architecture, don't use AI. You'll just generate chaos faster.**

---

## Stack used across case studies

| Layer | Technology | Why |
|-------|-----------|-----|
| Runtime | Node.js + TypeScript | Mature ecosystem, type safety as automatic guardrail, AI generates TS better than any other language |
| Frontend | Next.js (App Router) | Server Components, Vercel integration, largest component ecosystem |
| Backend | Hono | Edge-first, lightweight, composable middleware, end-to-end typing |
| Database | PostgreSQL (Neon) | Serverless, branching for preview deployments, pgvector for embeddings |
| ORM | Drizzle | Type-safe, explicit SQL, no binary engine, zero runtime overhead |
| Edge | Cloudflare Workers | No cold start, global deployment, predictable cost |
| AI | Anthropic Claude SDK | Agent orchestration with scoped context |
| Auth | NextAuth.js | Multi-tenant RBAC per organization |
| CI/CD | GitHub Actions | Native to repo, declarative workflows |
| Monitoring | Sentry | Real-time error tracking with full context |
| Deploy | Vercel + Cloudflare | Atomic deploys, automatic rollback, preview per PR |

---

## Key numbers from production

| Metric | Value |
|--------|-------|
| Restaurant CRM tests | 352 |
| Sports Analytics tests | 208 |
| Supply Chain tests | 89 |
| Min. coverage target | 70% |
| Agent response time | < 3s |
| Edge API latency | < 50ms globally |
| CI pipeline time | < 4 min |
| Lighthouse score | > 90 |

---

## License

[MIT](LICENSE)
