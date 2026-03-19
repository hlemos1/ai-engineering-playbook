# Real Prompts

Prompt patterns I use daily to build systems with AI. Nothing theoretical — all extracted from real development sessions.

---

## Persistent context: CLAUDE.md

Each repository has a CLAUDE.md file at the root. This file is read automatically by the AI at every session and defines:

- Stack and project conventions
- Architecture rules (separation of responsibilities, line limits per file)
- Mandatory workflow (plan before executing, tests required)
- Security checklist
- Product description and features

This eliminates the need to repeat context every conversation. The AI already knows the rules before you ask for anything.

---

## Pattern 1: System creation

Used when starting a new module or service.

```
Context: I'm building [system description].
Stack: [stack list].
Database schema: see src/db/schema.ts.

I need to implement the [name] module with:
- [function 1]: [description with input/output types]
- [function 2]: [description with input/output types]
- [function 3]: [description with input/output types]

Requirements:
- Input validation with Zod
- Error handling with custom types
- Maximum [N] lines per file
- Follow CLAUDE.md conventions

Deliver:
1. Service at src/services/[name].ts
2. Router/controller at src/routes/[name].ts
3. Unit tests at tests/[name].test.ts
```

The key is being specific about types, file locations, and constraints. The more precise the prompt, the fewer adjustment iterations.

---

## Pattern 2: Refactoring

Used when a file has grown too large or the structure is messy.

```
The file src/[path] has [N] lines and mixes responsibilities.

Refactor by separating into:
1. src/services/[name]-service.ts — business logic
2. src/utils/[name]-helpers.ts — pure helper functions
3. src/validators/[name]-validators.ts — Zod schemas

Keep the same public interface (exports).
Don't change behavior — only organize.
Update imports in all files that reference the original.
Run existing tests to confirm nothing broke.
```

Rule: refactoring never changes behavior. If you need to change behavior, that's a different task.

---

## Pattern 3: Test creation

Used after implementing a feature.

```
Write tests for src/services/[name].ts covering:

Success scenarios:
- [scenario 1]
- [scenario 2]

Error scenarios:
- [scenario with invalid input]
- [scenario with record not found]
- [scenario with permission denied]

Edge cases:
- [empty list]
- [pagination beyond total]
- [optional field missing]

Use vitest.
Only mock external dependencies (database, third-party APIs).
Don't use any — type everything.
Place at tests/services/[name].test.ts.
```

Covering error and edge scenarios is more important than success scenarios. Success usually works; what breaks in production are the edge cases.

---

## Pattern 4: Bug fix

Used when something is failing.

```
Bug: [description of wrong behavior]
Expected: [correct behavior]
File: src/[path]
Error: [stack trace or error message]

Analyze the code, identify the root cause, and propose the fix.

Before implementing:
1. Explain what's causing the bug
2. Describe the proposed fix
3. Wait for approval

After approval:
1. Implement the fix
2. Write a regression test that reproduces the original bug
3. Confirm existing tests still pass
```

Never accept "try this and see if it works." The AI must explain the root cause before fixing.

---

## Pattern 5: CI/CD setup

Used to configure continuous integration pipeline.

```
Configure GitHub Actions for this repository:

PR pipeline:
1. Checkout
2. Setup Node.js [version] with pnpm
3. Install dependencies (with cache)
4. Lint (pnpm lint)
5. Type check (pnpm typecheck)
6. Tests (pnpm test) with minimum [N]% coverage
7. Build (pnpm build)

Deploy pipeline (merge to main):
1. Everything above
2. Deploy to [Vercel/Cloudflare]
3. Post-deploy health check
4. Notification on failure

File: .github/workflows/ci.yml

Requirements:
- Cache node_modules and .next/cache
- Fail fast if lint or tests fail
- Maximum timeout of 10 minutes
- Secrets via GitHub Secrets (never hardcoded)
```

CI is not optional. Every repository going to production has a pipeline from day one.

---

## General observations

- Always include explicit constraints (line limits, naming conventions, file locations)
- Always request tests alongside implementation
- Never accept output without reviewing — especially database queries and authorization logic
- If the AI generates something you don't understand, ask for explanation before accepting
- Vague prompts generate vague code. Specificity is the difference between useful output and formatted garbage
