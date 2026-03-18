# Principios Fundamentais

Regras que guiam toda decisao tecnica antes, durante e depois da construcao.

---

## 1. Arquitetura antes de tudo

Nenhuma linha de codigo e escrita antes de existir um desenho claro do sistema. Isso inclui definir entidades, relacoes, fluxos de dados e pontos de integracao. Se voce pula essa etapa, a IA vai gerar codigo rapido — mas codigo que nao encaixa em lugar nenhum.

A arquitetura e o mapa. Sem mapa, velocidade e so uma forma mais rapida de se perder.

---

## 2. IA nao decide — IA executa

O papel da IA e gerar codigo, testes, refatoracoes e documentacao sob direcao humana. Quem define o que construir, como estruturar e qual trade-off aceitar e o humano. IA sem direcao gera output generico que parece funcional mas quebra em producao.

Toda vez que voce aceita sugestao de IA sem entender o que ela fez, voce esta acumulando divida tecnica invisivel.

---

## 3. Testes sao obrigatorios

Nao existe funcionalidade pronta sem teste. Teste unitario valida logica isolada, teste de integracao valida fluxo completo, teste E2E valida experiencia real. Se o teste nao existe, a funcionalidade nao esta entregue — esta pendente.

Cobertura minima de 70% nao e meta aspiracional. E requisito de entrada para producao.

---

## 4. Deploy faz parte do sistema

Um sistema que roda local nao existe. Deploy, CI/CD, monitoramento e alertas sao parte da arquitetura desde o dia zero. Se voce nao pensou em como vai para producao, voce nao desenhou um sistema — desenhou um experimento.

Pipeline quebrado e bug. Tratar como tal.

---

## 5. Simplicidade vence

Escolha a solucao mais simples que resolve o problema real. Microservicos, event sourcing e arquiteturas distribuidas existem para resolver problemas especificos — nao para impressionar. Se um monolito bem estruturado resolve, use um monolito.

Complexidade so se justifica quando a simplicidade ja falhou de forma documentada.

---

## 6. Problemas reais apenas

Nao construa funcionalidades especulativas. Cada feature precisa responder a pergunta: "qual problema de negocio isso resolve?" Se a resposta for vaga ou teorica, nao construa. Foco em entregar valor mensuravel para usuarios reais.

Demo que nao vira producao e desperdicio com aparencia de progresso.
