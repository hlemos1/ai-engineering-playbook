# Stack Recomendada

Cada escolha tem motivo. Nenhuma tecnologia esta aqui por hype.

---

## Runtime e linguagem

### Node.js + TypeScript

Por que: ecossistema maduro, tipagem estatica que previne erros antes da execucao, excelente suporte de IA para geracao de codigo. TypeScript e a linguagem que mais se beneficia de IA — o sistema de tipos funciona como guardrail automatico. Compartilhar a mesma linguagem entre frontend e backend reduz contexto mental.

---

## Backend

### Hono

Por que: framework leve, edge-first, compativel com Cloudflare Workers, Vercel Edge e Node.js. Sem opiniao excessiva, sem magica. API simples, middleware composavel, tipagem end-to-end. Substitui Express com menos overhead e mais portabilidade.

### Cloudflare Workers

Por que: deploy na edge com latencia minima, escala automatica, custo previsivel. Para APIs e microservicos que nao precisam de estado pesado, Workers sao a opcao mais eficiente. Cold start inexistente comparado com Lambda.

---

## Frontend

### Next.js + React

Por que: App Router com Server Components resolve SSR sem complexidade manual. Ecossistema React e o mais amplo — componentes, bibliotecas, talento disponivel. Vercel como plataforma de deploy torna o ciclo build-deploy trivial. Ideal para dashboards, paineis administrativos e landing pages.

---

## Banco de dados

### PostgreSQL (Neon)

Por que: PostgreSQL e o banco relacional mais completo — suporta JSON, full-text search, pgvector para embeddings. Neon oferece PostgreSQL serverless com branching (util para preview deployments e testes), escala para zero, e compativel com qualquer ORM. Custo zero para projetos iniciais.

### Drizzle ORM

Por que: type-safe, sem magica de reflection, queries SQL reais por baixo. Migrations declarativas, schema como codigo, zero overhead em runtime. Diferente de Prisma, nao gera cliente pesado nem depende de engine binaria. Voce escreve TypeScript e sabe exatamente qual SQL vai rodar.

---

## Deploy e infraestrutura

### Vercel

Por que: deploy automatico por push, preview por PR, edge functions nativas. Para aplicacoes Next.js, nao existe alternativa mais integrada. Analytics e Web Vitals inclusos.

### GitHub Actions

Por que: CI/CD nativo ao repositorio. Sem servidor separado, sem configuracao externa. Workflows declarativos, marketplace de actions, integracao direta com PRs e branches. Lint, testes, build e deploy em um unico lugar.

---

## Observabilidade

### Sentry

Por que: error tracking em tempo real com contexto completo — stack trace, breadcrumbs, session replay. Integracao nativa com Next.js, Hono e Workers. Sem Sentry, bugs em producao sao invisiveis ate o usuario reclamar. Com Sentry, voce sabe antes do usuario.

---

## Regras de stack

### 1. Escolheu, nao muda no meio do projeto

Trocar de ORM, framework ou banco no meio da construcao e a forma mais eficiente de desperdicar semanas. Avalie antes, decida, e mantenha. Se a escolha se mostrar errada, documente o motivo e migre de forma planejada — nunca no impulso.

### 2. Prefira simples ao complexo

Se PostgreSQL resolve, nao adicione Redis. Se um monolito atende, nao distribua. Se REST funciona, nao use GraphQL. Adicione complexidade apenas quando a simplicidade ja falhou de forma mensuravel.

### 3. Edge-first quando possivel

Comece pensando em edge deployment. Se a API pode rodar em Workers, rode. Se o middleware pode executar na edge, execute. Latencia menor, custo menor, escala automatica. Mova para servidor tradicional apenas quando o workload exigir (conexoes longas, processos pesados, estado em memoria).
