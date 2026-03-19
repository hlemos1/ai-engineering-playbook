# Recommended Stack

Every choice has a reason. Nothing is here for hype.

---

## Runtime and language

### Node.js + TypeScript

Why: mature ecosystem, static typing that prevents errors before execution, excellent AI support for code generation. TypeScript is the language that benefits most from AI — the type system works as an automatic guardrail. Sharing the same language between frontend and backend reduces mental context switching.

---

## Backend

### Hono

Why: lightweight framework, edge-first, compatible with Cloudflare Workers, Vercel Edge, and Node.js. No excessive opinions, no magic. Simple API, composable middleware, end-to-end typing. Replaces Express with less overhead and more portability.

### Cloudflare Workers

Why: edge deployment with minimal latency, automatic scaling, predictable cost. For APIs and microservices that don't need heavy state, Workers are the most efficient option. Non-existent cold start compared to Lambda.

---

## Frontend

### Next.js + React

Why: App Router with Server Components solves SSR without manual complexity. React's ecosystem is the broadest — components, libraries, available talent. Vercel as a deploy platform makes the build-deploy cycle trivial. Ideal for dashboards, admin panels, and landing pages.

---

## Database

### PostgreSQL (Neon)

Why: PostgreSQL is the most complete relational database — supports JSON, full-text search, pgvector for embeddings. Neon offers serverless PostgreSQL with branching (useful for preview deployments and testing), scales to zero, and is compatible with any ORM. Zero cost for initial projects.

### Drizzle ORM

Why: type-safe, no reflection magic, real SQL queries underneath. Declarative migrations, schema as code, zero runtime overhead. Unlike Prisma, it doesn't generate a heavy client or depend on a binary engine. You write TypeScript and know exactly which SQL will run.

---

## Deploy and infrastructure

### Vercel

Why: automatic deploy on push, preview per PR, native edge functions. For Next.js applications, there's no more integrated alternative. Analytics and Web Vitals included.

### GitHub Actions

Why: CI/CD native to the repository. No separate server, no external configuration. Declarative workflows, action marketplace, direct integration with PRs and branches. Lint, tests, build, and deploy in one place.

---

## Observability

### Sentry

Why: real-time error tracking with full context — stack trace, breadcrumbs, session replay. Native integration with Next.js, Hono, and Workers. Without Sentry, production bugs are invisible until the user complains. With Sentry, you know before the user does.

---

## Stack rules

### 1. Once chosen, don't switch mid-project

Swapping ORMs, frameworks, or databases mid-build is the most efficient way to waste weeks. Evaluate before, decide, and commit. If the choice proves wrong, document the reason and migrate in a planned way — never on impulse.

### 2. Prefer simple over complex

If PostgreSQL solves it, don't add Redis. If a monolith handles it, don't distribute. If REST works, don't use GraphQL. Add complexity only when simplicity has already failed in a measurable way.

### 3. Edge-first when possible

Start thinking about edge deployment. If the API can run on Workers, run it. If the middleware can execute at the edge, execute it. Lower latency, lower cost, automatic scaling. Move to a traditional server only when the workload demands it (long connections, heavy processing, in-memory state).
