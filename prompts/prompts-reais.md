# Prompts Reais

Padroes de prompt que uso no dia a dia para construir sistemas com IA. Nada teorico — tudo extraido de sessoes reais de desenvolvimento.

---

## Contexto persistente: CLAUDE.md

Cada repositorio tem um arquivo CLAUDE.md na raiz. Esse arquivo e lido automaticamente pela IA a cada sessao e define:

- Stack e convencoes do projeto
- Regras de arquitetura (separacao de responsabilidades, limite de linhas por arquivo)
- Workflow obrigatorio (planejar antes de executar, testes obrigatorios)
- Checklist de seguranca
- Descricao do produto e funcionalidades

Isso elimina a necessidade de repetir contexto a cada conversa. A IA ja sabe as regras antes de voce pedir qualquer coisa.

---

## Padrao 1: Criacao de sistema

Usado quando se inicia um novo modulo ou servico.

```
Contexto: estou construindo [descricao do sistema].
Stack: [lista da stack].
Schema do banco: ver src/db/schema.ts.

Preciso implementar o modulo de [nome] com:
- [funcao 1]: [descricao com tipos de entrada e saida]
- [funcao 2]: [descricao com tipos de entrada e saida]
- [funcao 3]: [descricao com tipos de entrada e saida]

Requisitos:
- Validacao de input com Zod
- Tratamento de erro com tipos customizados
- Maximo de [N] linhas por arquivo
- Seguir convencoes do CLAUDE.md

Entregue:
1. Service em src/services/[nome].ts
2. Router/controller em src/routes/[nome].ts
3. Testes unitarios em tests/[nome].test.ts
```

A chave e ser especifico sobre tipos, localizacao de arquivos e restricoes. Quanto mais preciso o prompt, menos iteracoes de ajuste.

---

## Padrao 2: Refatoracao

Usado quando um arquivo cresceu demais ou a estrutura esta confusa.

```
O arquivo src/[caminho] esta com [N] linhas e mistura responsabilidades.

Refatore separando em:
1. src/services/[nome]-service.ts — logica de negocio
2. src/utils/[nome]-helpers.ts — funcoes auxiliares puras
3. src/validators/[nome]-validators.ts — schemas Zod

Mantenha a mesma interface publica (exports).
Nao mude comportamento — apenas organize.
Atualize os imports em todos os arquivos que referenciam o original.
Rode os testes existentes para confirmar que nada quebrou.
```

Regra: refatoracao nunca muda comportamento. Se precisa mudar comportamento, e outra tarefa.

---

## Padrao 3: Criacao de testes

Usado apos implementar uma funcionalidade.

```
Escreva testes para src/services/[nome].ts cobrindo:

Cenarios de sucesso:
- [cenario 1]
- [cenario 2]

Cenarios de erro:
- [cenario com input invalido]
- [cenario com registro nao encontrado]
- [cenario com permissao negada]

Cenarios de borda:
- [lista vazia]
- [paginacao alem do total]
- [campo opcional ausente]

Use vitest.
Mock apenas dependencias externas (banco, APIs terceiras).
Nao use any — tipar tudo.
Coloque em tests/services/[nome].test.ts.
```

Cobrir cenarios de erro e borda e mais importante que cenarios de sucesso. Sucesso geralmente funciona; o que quebra em producao sao os edge cases.

---

## Padrao 4: Correcao de bug

Usado quando algo esta falhando.

```
Bug: [descricao do comportamento errado]
Esperado: [comportamento correto]
Arquivo: src/[caminho]
Erro: [stack trace ou mensagem de erro]

Analise o codigo, identifique a causa raiz e proponha a correcao.

Antes de implementar:
1. Explique o que esta causando o bug
2. Descreva a correcao proposta
3. Aguarde aprovacao

Apos aprovacao:
1. Implemente a correcao
2. Escreva teste de regressao que reproduz o bug original
3. Confirme que os testes existentes continuam passando
```

Nunca aceitar "tenta isso e ve se funciona". A IA deve explicar a causa raiz antes de corrigir.

---

## Padrao 5: Setup de CI/CD

Usado para configurar pipeline de integracao continua.

```
Configure GitHub Actions para este repositorio:

Pipeline de PR:
1. Checkout
2. Setup Node.js [versao] com pnpm
3. Install dependencies (com cache)
4. Lint (pnpm lint)
5. Type check (pnpm typecheck)
6. Testes (pnpm test) com cobertura minima de [N]%
7. Build (pnpm build)

Pipeline de deploy (merge na main):
1. Tudo acima
2. Deploy para [Vercel/Cloudflare]
3. Health check pos-deploy
4. Notificacao em caso de falha

Arquivo: .github/workflows/ci.yml

Requisitos:
- Cache de node_modules e .next/cache
- Fail fast se lint ou testes falharem
- Timeout maximo de 10 minutos
- Secrets via GitHub Secrets (nunca hardcoded)
```

CI nao e opcional. Todo repositorio que vai para producao tem pipeline desde o primeiro dia.

---

## Observacoes gerais

- Sempre incluir restricoes explicitas (limite de linhas, convencoes de nomenclatura, localizacao de arquivo)
- Sempre pedir testes junto com implementacao
- Nunca aceitar output sem revisar — especialmente queries de banco e logica de autorizacao
- Se a IA gerar algo que voce nao entende, peca explicacao antes de aceitar
- Prompts vagos geram codigo vago. Especificidade e a diferenca entre output util e lixo formatado
