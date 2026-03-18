# Caso: Supply Chain China-Brasil (Missao China HQ)

Sistema de gestao de operacoes de importacao China-Brasil.

---

## Problema

Operacoes de importacao da China para o Brasil sao gerenciadas em planilhas desconectadas. Cada embarque envolve dezenas de documentos, multiplos fornecedores, timelines diferentes e requisitos de compliance que mudam por categoria de produto. Nao existe visao unificada do status de cada operacao, e o risco de erro em documentacao aduaneira e alto — com consequencias financeiras reais.

---

## Solucao

Sistema de rastreamento unificado com:

- **Gestao de embarques** — cada importacao como entidade unica com timeline, documentos, custos e status
- **Compliance scoring** — score automatico por embarque baseado em completude de documentacao, historico do fornecedor e categoria de produto
- **Dashboard de operacoes** — visao consolidada de todos os embarques ativos, atrasados e finalizados
- **Gestao de fornecedores** — cadastro com historico de performance, lead times reais e confiabilidade
- **Alertas** — notificacao automatica para documentos pendentes, prazos proximos e anomalias de custo
- **Calculo de custos** — custo total de internacao (produto + frete + impostos + armazenagem) por embarque

---

## Arquitetura

```
Frontend: React + TypeScript + Tailwind CSS
Backend: Hono (API framework)
Runtime: Cloudflare Workers (edge deployment)
Banco: Cloudflare D1 (SQLite na edge)
Deploy: Cloudflare (wrangler)
CI: GitHub Actions (lint + testes + deploy)
```

### Decisoes de arquitetura

- **Cloudflare Workers + D1 sobre Node.js + PostgreSQL:** operacao leve, poucos usuarios simultaneos (equipe de importacao), sem necessidade de banco relacional pesado. D1 na edge oferece latencia minima para equipe distribuida entre Brasil e China (fusos horarios diferentes, acesso de ambos os lados).
- **Hono sobre Express:** framework feito para edge. Tipagem end-to-end, middleware composavel, compativel nativamente com Workers.
- **Sem ORM pesado:** D1 usa SQL direto com tipagem manual. Para schema simples e queries previssiveis, ORM adiciona overhead sem beneficio proporcional.
- **SPA sobre SSR:** dashboard interno, SEO irrelevante, usuarios sempre autenticados. SPA simplifica deploy e reduz dependencia de servidor.

---

## Resultados

- **89 testes** — unitarios e integracao
- **Edge deployment global** — latencia < 50ms tanto do Brasil quanto da China
- **Compliance scoring funcional** — score calculado automaticamente com base em 12 criterios documentados
- **Custo de infraestrutura proximo de zero** — Workers free tier cobre o volume de uso da equipe
- **Tempo de deploy: < 30 segundos** — push para main dispara deploy automatico via wrangler

---

## Aprendizados

1. **Edge-first funciona para ferramentas internas.** Equipes pequenas com usuarios distribuidos se beneficiam de latencia global uniforme. Nao precisa ser um sistema grande para justificar edge.

2. **Compliance scoring precisa de regras explicitas.** Cada criterio deve ser documentado, ponderado e auditavel. Score opaco nao gera confianca — a equipe precisa entender por que um embarque tem score baixo.

3. **D1 tem limitacoes.** Para queries complexas de agregacao e joins multiplos, SQLite na edge mostra limites. Para este caso de uso (CRUD + scoring + alertas), e suficiente. Para analytics pesado, migraria para PostgreSQL.

4. **Hono + Workers e a combinacao mais produtiva para APIs leves.** Deploy instantaneo, sem cold start, tipagem completa. Para microservicos e ferramentas internas, dificil justificar algo mais pesado.

5. **Operacoes de importacao tem sazonalidade.** O sistema precisa lidar com periodos de zero embarques e periodos de 20+ simultaneos sem degradar. Escala automatica do Workers resolve isso sem configuracao.
