# Workflow de Construcao

Processo em 7 etapas para sair de problema de negocio ate sistema em producao.

---

## O loop fundamental

```
IA gera -> humano valida -> IA ajusta -> producao
```

Esse loop se repete em cada etapa. A IA nunca opera sozinha. O humano nunca implementa manualmente o que a IA pode gerar mais rapido. Os dois juntos produzem mais que qualquer um isolado — mas so se o humano estiver no controle.

---

## Etapa 1: Definir o problema

Antes de qualquer codigo, definir em uma frase o problema real de negocio.

**Exemplo real (Restaurant CRM):**
"Restaurantes de delivery nao tem visibilidade clara sobre pedidos, margens e comportamento de clientes. Decisoes sao tomadas no achismo."

**Exemplo real (Missao China HQ):**
"Operacoes de importacao China-Brasil sao gerenciadas em planilhas fragmentadas. Nao existe score de compliance nem rastreamento unificado de embarques."

**Exemplo real (Cortex FC):**
"Analytics de futebol esta espalhado em ferramentas desconectadas. Scouts, analistas e diretoria nao compartilham a mesma base de dados."

A definicao do problema guia todas as decisoes seguintes. Se o problema muda, a arquitetura muda. Se o problema nao esta claro, pare e clarifique antes de prosseguir.

---

## Etapa 2: Arquitetura

Desenhar entidades, relacoes e fluxos antes de gerar qualquer codigo.

- Quais sao as entidades principais? (ex: Pedido, Cliente, Produto, Organizacao)
- Como se relacionam? (ex: Pedido pertence a Cliente, Organizacao tem muitos Usuarios)
- Quais sao os fluxos criticos? (ex: criar pedido -> notificar cozinha -> atualizar status -> feedback)
- Onde ficam os dados? (ex: PostgreSQL para dados relacionais, S3 para arquivos)
- Quem acessa o que? (ex: RBAC por organizacao, admin vs operador)

Resultado: diagrama de entidades, lista de endpoints, definicao de permissoes.

---

## Etapa 3: Selecao de stack

Escolher tecnologias com base no problema, nao em preferencia pessoal.

Perguntas guia:
- Precisa de SSR? -> Next.js
- API leve na edge? -> Hono + Workers
- Dados relacionais com tipagem? -> PostgreSQL + Drizzle
- Auth multi-tenant? -> NextAuth ou custom JWT
- Monitoramento? -> Sentry

Uma vez escolhida a stack, documentar no repositorio (README ou STACK.md) e nao mudar sem motivo documentado.

---

## Etapa 4: Orquestracao de IA

Como a IA participa do desenvolvimento:

### CLAUDE.md como contexto persistente

Cada repositorio tem um arquivo CLAUDE.md na raiz que define:
- Stack do projeto
- Convencoes de codigo
- Regras de arquitetura
- Workflow obrigatorio (planejar -> aprovar -> executar -> testar)
- Checklist de seguranca e qualidade

A IA le esse arquivo a cada sessao. Isso elimina repeticao de contexto e garante consistencia entre sessoes.

### Estrutura de prompts

Nao pedir "crie um sistema de pedidos". Pedir:

```
Dado o schema de banco definido em src/db/schema.ts,
implemente o service de pedidos em src/services/orders.ts com:
- createOrder(data: NewOrder): Promise<Order>
- getOrdersByOrganization(orgId: string, pagination: PaginationParams): Promise<PaginatedResult<Order>>
- updateOrderStatus(orderId: string, status: OrderStatus): Promise<Order>

Siga as convencoes do CLAUDE.md.
Inclua validacao de input com Zod.
Nao ultrapasse 200 linhas neste arquivo.
```

Contexto especifico, output previsivel, restricoes claras.

### Ciclo de iteracao

1. IA gera codigo
2. Humano revisa (nao aceita cegamente)
3. Humano aponta ajustes especificos
4. IA ajusta
5. Testes sao escritos (pela IA, validados pelo humano)
6. Commit atomico com mensagem descritiva

---

## Etapa 5: Iteracao e validacao

Cada funcionalidade segue o ciclo:

1. Implementar a feature minima
2. Testar manualmente
3. Escrever testes automatizados
4. Refatorar se necessario
5. Revisar seguranca (inputs validados? secrets protegidos? queries otimizadas?)
6. Commit

Nunca acumular multiplas features sem commit. Small releases, sempre.

---

## Etapa 6: Testes

Tres niveis obrigatorios:

### Unitarios
Validam logica isolada. Services, utils, helpers. Rapidos, sem dependencia externa.

### Integracao
Validam fluxo completo. API endpoint recebe request, processa, salva no banco, retorna response. Usam banco de teste real (nao mock).

### E2E (quando aplicavel)
Validam experiencia do usuario. Playwright para fluxos criticos: login, criar pedido, gerar relatorio.

**Exemplos reais de cobertura:**
- Restaurant CRM: 352 testes
- Missao China HQ: 89 testes
- Cortex FC: 208 testes

Meta minima: 70% de cobertura. Pipeline de CI falha se cobertura cair.

---

## Etapa 7: Deploy e monitoramento

### CI/CD com GitHub Actions

Pipeline padrao:
1. Lint (ESLint + Prettier)
2. Type check (tsc --noEmit)
3. Testes (vitest ou jest)
4. Build
5. Deploy (Vercel / Cloudflare)

### Monitoramento

- Sentry para error tracking
- Lighthouse CI para performance (meta: score > 90)
- Health check endpoint em toda API
- Alertas configurados para erros criticos

### Pos-deploy

- Verificar Sentry: novos erros?
- Verificar metricas: latencia, error rate
- Smoke test manual nos fluxos criticos
- Se algo quebrou: rollback, investigar, corrigir, re-deploy

---

## Resumo

O workflow nao e linear — e ciclico. Cada feature passa por todas as etapas. O sistema cresce de forma incremental, validada e monitorada. A IA acelera cada etapa, mas o humano garante que o resultado faz sentido para o negocio.
