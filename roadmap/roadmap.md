# Maturity Roadmap

Four maturity levels for building systems with AI. Each level adds capabilities on top of the previous one. Don't skip levels — the foundation supports everything that comes after.

---

## Level 1: Foundation

**CRUD + simple deploy**

Capabilities:
- Create basic REST endpoints (create, read, update, delete)
- Model entities and relationships in the database
- Automatic deploy to production (Vercel, Cloudflare)
- Use AI to generate code with specific prompts
- Atomic commits with descriptive messages

Completion criteria: you can go from defined problem to working system in production in less than a week, with AI generating the code under your direction.

---

## Level 2: Discipline

**Authentication + tests + quality**

Capabilities:
- Authentication and authorization (JWT, NextAuth, sessions)
- Basic RBAC (admin, user, operator)
- Unit and integration tests with coverage > 70%
- Complete CI/CD (lint + typecheck + tests + build + deploy)
- Input validation with typed schemas (Zod)
- Structured error handling (not generic try/catch)

Completion criteria: every system you build has functional auth, automated tests, and a CI pipeline that blocks merge if anything fails. No feature exists without a test.

---

## Level 3: Robustness

**Multi-tenancy + observability + security**

Capabilities:
- Real multi-tenancy (data isolation per organization at query level)
- Sentry or equivalent for production error tracking
- Security headers configured (CSP, HSTS, X-Frame-Options)
- Rate limiting on public endpoints
- Performance monitoring (Lighthouse CI, Web Vitals)
- Reversible and versioned migrations
- Secrets in environment variables, never in code
- Structured logs without sensitive data
- Health check endpoints

Completion criteria: your system is multi-tenant, monitored, secure, and auditable. You know when something breaks before the user reports it. One client's data never leaks to another.

---

## Level 4: Ecosystem

**AI automation + system integration + network thinking**

Capabilities:
- AI agents with defined scope operating within the system
- Integration across multiple systems (CRM + analytics + supply chain + finance)
- Automated workflow orchestration (N8N, background jobs)
- RAG with embeddings for proprietary context
- Multi-system unified identity (SSO or shared token)
- Business metrics derived from multi-system data
- Ability to add new modules without rewriting existing ones
- Ecosystem thinking: each new system strengthens the existing ones

Completion criteria: you don't build isolated projects — you build connected systems that feed each other. Data flows between systems. AI agents operate on real data with restricted scope. The ecosystem generates more value than the sum of its parts.

---

## End goal

It's not about building a system. It's about building the capability to build systems — fast, with quality, monitored, and connected.

Each new project is faster than the last because:
- The stack is defined
- The patterns are documented
- The workflow is internalized
- The AI is calibrated with persistent context (CLAUDE.md)
- Past mistakes are recorded and don't repeat

The goal isn't speed. It's consistency. Speed is a consequence of a mature process.
