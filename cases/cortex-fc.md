# Caso: Cortex FC

Sistema de analytics esportivo com IA para futebol.

---

## Problema

Analytics de futebol esta fragmentado entre planilhas, videos, relatorios manuais e ferramentas desconectadas. Scouts fazem analises que nao chegam ao diretor tecnico. Analistas de desempenho usam metricas diferentes das do departamento medico. Relatorios de adversarios sao feitos em formatos incompativeis. O resultado: decisoes taticas e de contratacao baseadas em informacao incompleta ou desatualizada.

---

## Solucao

Plataforma unificada de analytics com 7 agentes de IA especializados:

- **Agente de Scouting** — analise de jogadores potenciais com base em metricas estatisticas, video highlights e compatibilidade tatica
- **Agente de Analise Tatica** — decomposicao de formacoes, padroes de jogo e transicoes ofensivas/defensivas
- **Agente de Performance** — monitoramento de carga de treino, fadiga acumulada e risco de lesao
- **Agente de Adversarios** — relatorios automaticos sobre proximos oponentes com pontos fracos e padroes identificados
- **Agente de Relatorios** — geracao automatica de relatorios consolidados para comissao tecnica e diretoria
- **Agente de Dados** — integracao e normalizacao de dados de multiplas fontes (APIs de stats, GPS, video)
- **Agente de Estrategia** — simulacao de cenarios taticos e recomendacoes baseadas em dados historicos

Cada agente opera de forma independente mas compartilha a mesma base de dados. Resultados sao combinados em dashboards por papel (scout, analista, diretor, treinador).

---

## Arquitetura

```
Frontend: Next.js 14 (App Router) + TypeScript + Tailwind CSS
ORM: Drizzle ORM
Banco: PostgreSQL (Neon)
IA: Anthropic Claude SDK (orquestracao de agentes)
Auth: NextAuth.js com RBAC multi-organizacao
PWA: Service workers + cache offline
Deploy: Vercel
CI: GitHub Actions
```

### Decisoes de arquitetura

- **Multi-tenancy por organizacao:** cada clube e uma organizacao isolada. Dados de scouting do Clube A nao sao visiveis para o Clube B. Isolamento total a nivel de query (org_id em toda tabela).
- **RBAC granular:** scout ve scouting e relatorios. Analista ve performance e tatica. Diretor ve tudo. Treinador ve tatica e adversarios. Cada papel tem permissoes especificas por modulo.
- **Offline-first PWA:** analistas trabalham em estadios, centros de treinamento e viagens — frequentemente sem internet estavel. O sistema funciona offline com sync quando reconecta.
- **Anthropic SDK para agentes:** cada agente e uma chain com contexto especifico (system prompt + dados relevantes + formato de output). Nao e um modelo generico — cada agente sabe exatamente seu escopo.
- **Separacao agente/dados:** agentes de IA nao acessam banco diretamente. Eles recebem dados pre-processados e retornam analises estruturadas. O service layer faz a mediacao.

---

## Resultados

- **208 testes** — unitarios, integracao e E2E para fluxos criticos
- **RBAC multi-organizacao validado** — testes especificos para cada combinacao de papel/recurso/organizacao
- **PWA funcional offline** — cache de dados recentes, formularios de input com sync posterior
- **7 agentes operacionais** — cada um com prompt especializado, validacao de output e fallback
- **Pipeline CI completa** — lint, typecheck, testes, build, deploy automatico
- **Tempo de resposta dos agentes < 3 segundos** — otimizado com cache de contexto e prompts enxutos

---

## Aprendizados

1. **Multi-agente exige orquestracao rigida.** Sete agentes compartilhando dados criam possibilidades de inconsistencia. Cada agente deve ter input/output tipado e validado. Agente que retorna formato inesperado e bug — tratar como tal.

2. **RBAC multi-tenant e a feature mais critica e menos visivel.** Se um scout de um clube ve dados de outro, o produto morre. Testes de permissao sao os mais importantes do sistema.

3. **Offline-first nao e feature — e requisito de infra.** Em contexto esportivo, a internet nao e confiavel. Se o sistema nao funciona offline, nao funciona no campo.

4. **Agentes de IA precisam de escopo restrito.** Agente que faz tudo nao faz nada bem. Cada agente com um papel claro, dados limitados e formato de output definido produz resultados consistentes.

5. **Separar agente de acesso a dados previne alucinacao.** O agente recebe dados reais e analisa — nao busca dados por conta propria. Isso elimina a possibilidade de inventar estatisticas que nao existem.
