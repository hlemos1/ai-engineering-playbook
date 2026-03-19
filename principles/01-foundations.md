# Core Principles

Rules that guide every technical decision before, during, and after building.

---

## 1. Architecture before everything

No line of code is written before a clear system design exists. This includes defining entities, relationships, data flows, and integration points. If you skip this step, AI will generate code fast — but code that doesn't fit anywhere.

Architecture is the map. Without a map, speed is just a faster way to get lost.

---

## 2. AI doesn't decide — AI executes

AI's role is to generate code, tests, refactors, and documentation under human direction. The human defines what to build, how to structure it, and which trade-offs to accept. AI without direction generates generic output that looks functional but breaks in production.

Every time you accept an AI suggestion without understanding what it did, you're accumulating invisible technical debt.

---

## 3. Tests are mandatory

There's no finished feature without a test. Unit tests validate isolated logic, integration tests validate complete flows, E2E tests validate real experience. If the test doesn't exist, the feature isn't delivered — it's pending.

70% minimum coverage is not an aspirational goal. It's a production entry requirement.

---

## 4. Deploy is part of the system

A system that only runs locally doesn't exist. Deploy, CI/CD, monitoring, and alerts are part of the architecture from day zero. If you haven't thought about how it goes to production, you haven't designed a system — you've designed an experiment.

A broken pipeline is a bug. Treat it as one.

---

## 5. Simplicity wins

Choose the simplest solution that solves the real problem. Microservices, event sourcing, and distributed architectures exist to solve specific problems — not to impress. If a well-structured monolith works, use a monolith.

Complexity is only justified when simplicity has already failed in a documented way.

---

## 6. Real problems only

Don't build speculative features. Every feature needs to answer: "what business problem does this solve?" If the answer is vague or theoretical, don't build it. Focus on delivering measurable value to real users.

A demo that never becomes production is waste with the appearance of progress.
