# Common Mistakes

Failure patterns that repeat when using AI to build systems. All observed in practice.

---

## 1. Starting with code

The most frequent mistake. Opening the editor and asking AI to "create a system for X" without having defined the problem, entities, flows, or architecture. The result is code that looks functional but doesn't solve the real problem — or solves the wrong one.

**Fix:** always start with the problem definition in one sentence. Then architecture. Then code.

---

## 2. Generic prompts

"Create a user CRUD." This generates generic code that needs to be rewritten. Without specifying types, validations, error handling, file locations, and constraints, the AI's output is an educated guess.

**Fix:** specific prompts with input/output types, exact file locations, size constraints, and project conventions. The more context, the fewer iterations.

---

## 3. Not validating AI output

Accepting generated code without reading it. "If it compiles, it works." This creates invisible technical debt — incorrect logic, inefficient queries, security vulnerabilities, inconsistent patterns. The problem only surfaces in production, when the cost of fixing is 10x higher.

**Fix:** review all generated code. If you don't understand what the AI wrote, ask for an explanation before accepting. If the explanation doesn't make sense, reject it.

---

## 4. Overly complex stack

Microservices for a CRUD. Kafka for 10 requests per minute. Kubernetes for an API that fits in a Worker. Premature complexity is the most expensive way to look sophisticated without delivering value.

**Fix:** use the simplest stack that solves the problem. Well-structured monolith > poorly managed microservices. Pure PostgreSQL > PostgreSQL + Redis + MongoDB + Elasticsearch when the volume doesn't justify it.

---

## 5. No tests

"I'll add tests later." Later never comes. Code without tests is code nobody trusts — not even you. Refactoring becomes Russian roulette. Deploy becomes a bet. Every change might break something and nobody will know.

**Fix:** tests are part of the feature, not a separate step. A feature without tests isn't delivered. CI that doesn't run tests is decorative CI.

---

## 6. Not understanding what was generated

The AI generates 200 lines of code. You take a quick look, seems good, commit it. Three days later something breaks and you don't know why — because you never knew how it worked.

**Fix:** if you can't explain what each part of the code does, it's not ready to be committed. Ask the AI to explain. Read the documentation for the libraries used. Understand before accepting.

---

## 7. No CI/CD

Code goes to production via manual deploy. No lint, no typecheck, no automated tests in the pipeline. Every deploy is an act of faith. Type errors pass, broken imports pass, failing tests pass.

**Fix:** GitHub Actions from the first commit. Lint + typecheck + tests + build. If any step fails, merge blocked. No exceptions, no bypass.

---

## 8. Treating AI as magic

Believing AI will "solve everything" if you give it the right prompt. This leads to delegating architectural decisions to AI, accepting suggestions without questioning, and losing control of the system you're building.

**Fix:** AI is a tool, not an architect. It executes well what you define well. It executes poorly what you define poorly. Output quality is directly proportional to input quality — and the input is your responsibility.
