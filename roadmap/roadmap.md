# Roadmap de Maturidade

Quatro niveis de maturidade na construcao de sistemas com IA. Cada nivel adiciona capacidades sobre o anterior. Nao pule niveis — a base sustenta tudo que vem depois.

---

## Nivel 1: Fundacao

**CRUD + deploy simples**

Capacidades:
- Criar endpoints REST basicos (create, read, update, delete)
- Modelar entidades e relacoes no banco de dados
- Fazer deploy automatico para producao (Vercel, Cloudflare)
- Usar IA para gerar codigo com prompts especificos
- Commit atomico com mensagens descritivas

Criterio de conclusao: voce consegue sair de problema definido ate sistema funcionando em producao em menos de uma semana, com IA gerando o codigo sob sua direcao.

---

## Nivel 2: Disciplina

**Autenticacao + testes + qualidade**

Capacidades:
- Autenticacao e autorizacao (JWT, NextAuth, sessoes)
- RBAC basico (admin, usuario, operador)
- Testes unitarios e de integracao com cobertura > 70%
- CI/CD completo (lint + typecheck + testes + build + deploy)
- Validacao de input com schemas tipados (Zod)
- Tratamento de erro estruturado (nao try/catch generico)

Criterio de conclusao: todo sistema que voce constroi tem auth funcional, testes automatizados e pipeline de CI que bloqueia merge se algo falha. Nao existe feature sem teste.

---

## Nivel 3: Robustez

**Multi-tenancy + observabilidade + seguranca**

Capacidades:
- Multi-tenancy real (isolamento de dados por organizacao a nivel de query)
- Sentry ou equivalente para error tracking em producao
- Security headers configurados (CSP, HSTS, X-Frame-Options)
- Rate limiting em endpoints publicos
- Monitoramento de performance (Lighthouse CI, Web Vitals)
- Migrations reversiveis e versionadas
- Secrets em variaveis de ambiente, nunca no codigo
- Logs estruturados sem dados sensiveis
- Health check endpoints

Criterio de conclusao: seu sistema e multi-tenant, monitorado, seguro e auditavel. Voce sabe quando algo quebra antes do usuario reportar. Dados de um cliente nunca vazam para outro.

---

## Nivel 4: Ecossistema

**Automacao com IA + integracao de sistemas + pensamento de rede**

Capacidades:
- Agentes de IA com escopo definido operando dentro do sistema
- Integracao entre multiplos sistemas (CRM + analytics + supply chain + financeiro)
- Orquestracao de workflows automatizados (N8N, jobs em background)
- RAG com embeddings para contexto proprietario
- Multi-sistema com identidade unificada (SSO ou token compartilhado)
- Metricas de negocio derivadas de dados de multiplos sistemas
- Capacidade de adicionar novos modulos sem reescrever os existentes
- Pensamento de ecossistema: cada sistema novo fortalece os existentes

Criterio de conclusao: voce nao constroi projetos isolados — constroi sistemas conectados que se alimentam mutuamente. Dados fluem entre sistemas. Agentes de IA operam sobre dados reais com escopo restrito. O ecossistema gera valor maior que a soma das partes.

---

## Meta final

Nao e construir um sistema. E construir a capacidade de construir sistemas — rapido, com qualidade, monitorados e conectados.

Cada projeto novo e mais rapido que o anterior porque:
- A stack esta definida
- Os padroes estao documentados
- O workflow esta internalizado
- A IA esta calibrada com contexto persistente (CLAUDE.md)
- Os erros do passado estao registrados e nao se repetem

O objetivo nao e velocidade. E consistencia. Velocidade e consequencia de processo maduro.
