# AI Engineering Playbook

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-100%25-007ACC?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Next.js](https://img.shields.io/badge/Built_with-Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)](https://nextjs.org/)
[![Anthropic](https://img.shields.io/badge/AI-Anthropic_Claude-191919?style=flat-square&logo=anthropic&logoColor=white)](https://anthropic.com)

> How to build real systems using AI as an execution layer. An operational manual with real case studies, prompt patterns, and production numbers.

This isn't theory. Every pattern here comes from shipping 21 active systems with CI/CD, 1,500+ automated tests, and 4 apps running in production — built using AI as the engineering execution layer.

---

## 📖 What's inside

### 1. The Architecture Mindset
> Your job is to architect. Not to code.

- Define the problem clearly before any prompt
- Structure business logic as specifications, not instructions
- Let AI agents execute, test, and iterate

### 2. Prompt Patterns That Ship

**The Specification Pattern**
```
Context: [what the system does, who uses it]
Constraint: [tech stack, patterns, existing code]
Task: [exactly what to build]
Output: [file structure, tests required, CI integration]
```

**The Validation Loop**
```
1. Agent writes code
2. CI runs (GitHub Actions)
3. Tests pass/fail → agent fixes
4. Lighthouse/Sentry gate → agent optimizes
5. You review architecture, not code
```

### 3. Production Stack

Every system built with this playbook follows the same stack:

| Layer | Choice | Why |
|---|---|---|
| Framework | Next.js 16 (App Router) | Full-stack, edge-ready, TypeScript-native |
| Database | Neon (PostgreSQL serverless) | Scales to zero, branch-based dev |
| ORM | Drizzle | Type-safe, SQL-close, fast migrations |
| Auth | NextAuth.js v5 | Production-ready, minimal config |
| CI/CD | GitHub Actions | Native, free, integrates with everything |
| Observability | Sentry + Lighthouse CI | Error tracking + perf gates |
| Deploy | Vercel | Zero-config, preview deploys |

### 4. CI/CD Template

Every repo gets this pipeline on day 1:

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pnpm install
      - run: pnpm lint
      - run: pnpm test
      - run: pnpm build
  lighthouse:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: treosh/lighthouse-ci-action@v10
```

### 5. Real Numbers

From the CORTEX3 production ecosystem:

- **21 active projects** with this exact CI/CD setup
- **1,500+ automated tests** across the portfolio
- **4 apps in production** on Vercel + Neon + Cloudflare
- **100% TypeScript** — no exceptions
- **4 repos with Sentry** monitoring production errors
- **4 repos with Lighthouse CI** enforcing performance budgets

---

## 🏗️ Systems built with this playbook

| System | Stack | Live |
|---|---|---|
| [Restaurant CRM](https://github.com/hlemos1/restaurant-crm) | Next.js, Drizzle, Sentry, Playwright | ✅ |
| [Cortex FC](https://github.com/hlemos1/cortex-fc) | Next.js, 7 AI agents, Anthropic | ✅ |
| [Grupo +351](https://github.com/hlemos1/grupo-351) | Next.js, Prisma, Stripe, Redis | ✅ |
| [Missao China HQ](https://github.com/hlemos1/missao-china-hq) | Hono, Cloudflare Workers | ✅ |
| [CasaRao Luz](https://github.com/hlemos1/casarao-luz) | Next.js, Drizzle, Lighthouse CI | ✅ |
| [Mundo Rao](https://github.com/hlemos1/mundao) | React, Hono, Cloudflare Workers | ✅ |

---

## 🧠 Core Principle

> AI doesn't replace engineering. AI accelerates engineering — if you know what you're doing.

The difference between chaos and production is architecture. Prompts without architecture ship bugs. Architecture with AI ships systems.

---

## 🤝 Author

**Henrique Lemos** — AI-driven systems architect  
Co-founder Grupo Rao (200+ units) · Building CORTEX3  
[GitHub](https://github.com/hlemos1) · [LinkedIn](https://linkedin.com/in/henrique-lemos-39712b22b)

---

## 📄 License

MIT © [Henrique Lemos](https://github.com/hlemos1)
