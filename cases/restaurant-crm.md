# Caso: Restaurant CRM

Sistema de gestao operacional para redes de delivery.

---

## Problema

Restaurantes de delivery operam sem visibilidade real sobre o negocio. Pedidos sao rastreados em plataformas fragmentadas, margens nao sao calculadas por produto, comportamento de cliente nao e analisado, e decisoes operacionais sao tomadas no achismo. Quanto maior a rede, maior o buraco negro de informacao.

---

## Solucao

CRM operacional completo com:

- **Dashboards em tempo real** — pedidos, faturamento, ticket medio, margens por produto e por loja
- **Rastreamento de pedidos** — status do pedido do recebimento ate entrega, com timestamps em cada etapa
- **Gestao de clientes** — historico de pedidos, frequencia, valor total gasto, segmentacao automatica
- **Gamificacao** — sistema de fidelidade com niveis, pontos e recompensas baseado em comportamento real
- **Multi-loja** — cada unidade ve seus dados, gestao central ve tudo agregado
- **Relatorios** — exportacao de dados, comparativos entre periodos, alertas de anomalia

---

## Arquitetura

```
Frontend: Next.js 14 (App Router) + TypeScript + Tailwind CSS
ORM: Drizzle ORM (type-safe, SQL explicito)
Banco: PostgreSQL via Neon (serverless, branching para previews)
Auth: NextAuth.js com RBAC por organizacao
Deploy: Vercel (preview por PR, production na main)
Monitoramento: Sentry (error tracking) + Lighthouse CI (performance)
CI: GitHub Actions (lint + typecheck + testes + build + deploy)
```

### Decisoes de arquitetura

- **Drizzle sobre Prisma:** queries explicitas, sem engine binaria, melhor debug. Em sistema com queries complexas de agregacao (margens, metricas), saber exatamente qual SQL roda e critico.
- **Neon sobre Supabase:** branching nativo permite que cada PR tenha seu proprio banco de teste. Sem conflito entre ambientes.
- **NextAuth com RBAC customizado:** cada organizacao (restaurante) e isolada. Usuario admin da rede ve todas as unidades. Gerente de loja ve so a sua.
- **Server Components por padrao:** dados carregam no servidor, menos JavaScript no cliente, melhor performance em dispositivos moveis (operadores usam celular frequentemente).

---

## Resultados

- **352 testes** — unitarios, integracao e E2E
- **CI sempre verde** — pipeline completa roda em menos de 4 minutos
- **Sentry integrado** — erros em producao rastreados com contexto completo
- **Lighthouse CI** — score acima de 90 em performance, acessibilidade e SEO
- **Zero downtime** — deploys atomicos via Vercel com rollback automatico
- **Multi-tenancy funcional** — dados isolados por organizacao, testado com multiplas unidades simultaneas

---

## Aprendizados

1. **Gamificacao parece simples, e complexa.** Regras de pontuacao, niveis, recompensas e edges cases (cancelamento de pedido que ja deu pontos, expiracao de pontos) exigem modelagem cuidadosa. Nao subestimar.

2. **Dashboards sao a feature mais politica.** Cada stakeholder quer metricas diferentes. Definir os indicadores antes de construir o dashboard evita retrabalho infinito.

3. **RBAC multi-tenant precisa de testes extensivos.** A pergunta "usuario X consegue ver dados da organizacao Y?" precisa ser respondida por teste automatizado, nao por revisao manual.

4. **Drizzle + Neon e uma combinacao eficiente.** Migrations como codigo, branching por PR, type-safety end-to-end. O ciclo de desenvolvimento ficou significativamente mais rapido.

5. **Lighthouse CI no pipeline pega regressoes de performance antes do deploy.** Bundle que cresceu? Imagem sem otimizacao? O CI avisa antes de ir para producao.
