# Build Workflow

7-step process to go from business problem to production system.

---

## The fundamental loop

```
AI generates -> human validates -> AI adjusts -> production
```

This loop repeats at every stage. AI never operates alone. The human never manually implements what AI can generate faster. Together they produce more than either in isolation — but only if the human is in control.

---

## Step 1: Define the problem

Before any code, define the real business problem in one sentence.

**Real example (Restaurant CRM):**
"Delivery restaurants have no clear visibility into orders, margins, and customer behavior. Decisions are made by guesswork."

**Real example (Supply Chain China-Brazil):**
"China-Brazil import operations are managed in fragmented spreadsheets. There's no compliance score or unified shipment tracking."

**Real example (Sports Analytics):**
"Football analytics is scattered across disconnected tools. Scouts, analysts, and board members don't share the same data foundation."

The problem definition guides every decision that follows. If the problem changes, the architecture changes. If the problem isn't clear, stop and clarify before proceeding.

---

## Step 2: Architecture

Design entities, relationships, and flows before generating any code.

- What are the main entities? (e.g., Order, Customer, Product, Organization)
- How do they relate? (e.g., Order belongs to Customer, Organization has many Users)
- What are the critical flows? (e.g., create order -> notify kitchen -> update status -> feedback)
- Where does data live? (e.g., PostgreSQL for relational data, S3 for files)
- Who accesses what? (e.g., RBAC per organization, admin vs operator)

Output: entity diagram, endpoint list, permission definitions.

---

## Step 3: Stack selection

Choose technologies based on the problem, not personal preference.

Guiding questions:
- Need SSR? -> Next.js
- Lightweight edge API? -> Hono + Workers
- Relational data with typing? -> PostgreSQL + Drizzle
- Multi-tenant auth? -> NextAuth or custom JWT
- Monitoring? -> Sentry

Once the stack is chosen, document it in the repository (README or STACK.md) and don't change without a documented reason.

---

## Step 4: AI orchestration

How AI participates in development:

### CLAUDE.md as persistent context

Each repository has a CLAUDE.md file at the root that defines:
- Project stack
- Code conventions
- Architecture rules
- Mandatory workflow (plan -> approve -> execute -> test)
- Security and quality checklist

The AI reads this file at every session. This eliminates context repetition and ensures consistency across sessions.

### Prompt structure

Don't ask "create an order system." Ask:

```
Given the database schema defined in src/db/schema.ts,
implement the orders service in src/services/orders.ts with:
- createOrder(data: NewOrder): Promise<Order>
- getOrdersByOrganization(orgId: string, pagination: PaginationParams): Promise<PaginatedResult<Order>>
- updateOrderStatus(orderId: string, status: OrderStatus): Promise<Order>

Follow CLAUDE.md conventions.
Include input validation with Zod.
Don't exceed 200 lines in this file.
```

Specific context, predictable output, clear constraints.

### Iteration cycle

1. AI generates code
2. Human reviews (never accepts blindly)
3. Human points out specific adjustments
4. AI adjusts
5. Tests are written (by AI, validated by human)
6. Atomic commit with descriptive message

---

## Step 5: Iteration and validation

Each feature follows the cycle:

1. Implement the minimum feature
2. Test manually
3. Write automated tests
4. Refactor if needed
5. Security review (inputs validated? secrets protected? queries optimized?)
6. Commit

Never accumulate multiple features without committing. Small releases, always.

---

## Step 6: Testing

Three mandatory levels:

### Unit
Validate isolated logic. Services, utils, helpers. Fast, no external dependencies.

### Integration
Validate complete flow. API endpoint receives request, processes, saves to database, returns response. Use real test database (not mocks).

### E2E (when applicable)
Validate user experience. Playwright for critical flows: login, create order, generate report.

**Real coverage examples:**
- Restaurant CRM: 352 tests
- Supply Chain China-Brazil: 89 tests
- Sports Analytics (Cortex FC): 208 tests

Minimum target: 70% coverage. CI pipeline fails if coverage drops.

---

## Step 7: Deploy and monitoring

### CI/CD with GitHub Actions

Standard pipeline:
1. Lint (ESLint + Prettier)
2. Type check (tsc --noEmit)
3. Tests (vitest or jest)
4. Build
5. Deploy (Vercel / Cloudflare)

### Monitoring

- Sentry for error tracking
- Lighthouse CI for performance (target: score > 90)
- Health check endpoint on every API
- Alerts configured for critical errors

### Post-deploy

- Check Sentry: new errors?
- Check metrics: latency, error rate
- Manual smoke test on critical flows
- If something broke: rollback, investigate, fix, re-deploy

---

## Summary

The workflow isn't linear — it's cyclical. Each feature goes through every step. The system grows incrementally, validated and monitored. AI accelerates each step, but the human ensures the result makes sense for the business.
