# Case: Sports Analytics with AI (Cortex FC)

AI-powered sports analytics system for football.

---

## Problem

Football analytics is fragmented across spreadsheets, videos, manual reports, and disconnected tools. Scouts produce analyses that never reach the technical director. Performance analysts use different metrics than the medical department. Opposition reports are created in incompatible formats. The result: tactical and recruitment decisions based on incomplete or outdated information.

---

## Solution

Unified analytics platform with 7 specialized AI agents:

- **Scouting Agent** — analysis of potential players based on statistical metrics, video highlights, and tactical compatibility
- **Tactical Analysis Agent** — decomposition of formations, game patterns, and offensive/defensive transitions
- **Performance Agent** — monitoring of training load, accumulated fatigue, and injury risk
- **Opposition Agent** — automatic reports on upcoming opponents with identified weaknesses and patterns
- **Reporting Agent** — automatic generation of consolidated reports for coaching staff and board
- **Data Agent** — integration and normalization of data from multiple sources (stats APIs, GPS, video)
- **Strategy Agent** — simulation of tactical scenarios and recommendations based on historical data

Each agent operates independently but shares the same database. Results are combined in dashboards by role (scout, analyst, director, coach).

---

## Architecture

```
Frontend: Next.js 14 (App Router) + TypeScript + Tailwind CSS
ORM: Drizzle ORM
Database: PostgreSQL (Neon)
AI: Anthropic Claude SDK (agent orchestration)
Auth: NextAuth.js with multi-organization RBAC
PWA: Service workers + offline cache
Deploy: Vercel
CI: GitHub Actions
```

### Architecture decisions

- **Multi-tenancy per organization:** each club is an isolated organization. Club A's scouting data is not visible to Club B. Total isolation at query level (org_id on every table).
- **Granular RBAC:** scout sees scouting and reports. Analyst sees performance and tactics. Director sees everything. Coach sees tactics and opposition. Each role has specific permissions per module.
- **Offline-first PWA:** analysts work in stadiums, training centers, and during travel — frequently without stable internet. The system works offline with sync on reconnection.
- **Anthropic SDK for agents:** each agent is a chain with specific context (system prompt + relevant data + output format). It's not a generic model — each agent knows exactly its scope.
- **Agent/data separation:** AI agents don't access the database directly. They receive pre-processed data and return structured analyses. The service layer mediates.

---

## Results

- **208 tests** — unit, integration, and E2E for critical flows
- **Multi-organization RBAC validated** — specific tests for each role/resource/organization combination
- **Functional offline PWA** — cache of recent data, input forms with deferred sync
- **7 operational agents** — each with specialized prompt, output validation, and fallback
- **Complete CI pipeline** — lint, typecheck, tests, build, automatic deploy
- **Agent response time < 3 seconds** — optimized with context caching and lean prompts

---

## Lessons learned

1. **Multi-agent requires strict orchestration.** Seven agents sharing data create possibilities for inconsistency. Each agent must have typed and validated input/output. An agent that returns an unexpected format is a bug — treat it as one.

2. **Multi-tenant RBAC is the most critical and least visible feature.** If a scout from one club sees another club's data, the product is dead. Permission tests are the most important tests in the system.

3. **Offline-first isn't a feature — it's an infrastructure requirement.** In sports contexts, internet is unreliable. If the system doesn't work offline, it doesn't work in the field.

4. **AI agents need restricted scope.** An agent that does everything does nothing well. Each agent with a clear role, limited data, and defined output format produces consistent results.

5. **Separating agents from data access prevents hallucination.** The agent receives real data and analyzes it — it doesn't fetch data on its own. This eliminates the possibility of inventing statistics that don't exist.
