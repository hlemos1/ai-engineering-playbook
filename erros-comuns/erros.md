# Erros Comuns

Padroes de falha que se repetem quando se usa IA para construir sistemas. Todos observados na pratica.

---

## 1. Comecar pelo codigo

O erro mais frequente. Abrir o editor e pedir para a IA "criar um sistema de X" sem ter definido o problema, as entidades, os fluxos ou a arquitetura. O resultado e codigo que parece funcional mas nao resolve o problema real — ou resolve o problema errado.

**Correcao:** sempre comecar pela definicao do problema em uma frase. Depois arquitetura. Depois codigo.

---

## 2. Prompts genericos

"Cria um CRUD de usuarios." Isso gera codigo generico que precisa ser reescrito. Sem especificar tipos, validacoes, tratamento de erro, localizacao de arquivos e restricoes, o output da IA e um chute educado.

**Correcao:** prompts especificos com tipos de entrada/saida, localizacao exata dos arquivos, restricoes de tamanho e convencoes do projeto. Quanto mais contexto, menos iteracoes.

---

## 3. Nao validar output da IA

Aceitar codigo gerado sem ler. "Se compila, funciona." Isso cria divida tecnica invisivel — logica incorreta, queries ineficientes, vulnerabilidades de seguranca, patterns inconsistentes. O problema so aparece em producao, quando o custo de correcao e 10x maior.

**Correcao:** revisar todo codigo gerado. Se voce nao entende o que a IA escreveu, peca explicacao antes de aceitar. Se a explicacao nao faz sentido, rejeite.

---

## 4. Stack complexa demais

Microservicos para um CRUD. Kafka para 10 requisicoes por minuto. Kubernetes para uma API que cabe em um Worker. Complexidade prematura e a forma mais cara de parecer sofisticado sem entregar valor.

**Correcao:** usar a stack mais simples que resolve o problema. Monolito bem estruturado > microservicos mal gerenciados. PostgreSQL puro > PostgreSQL + Redis + MongoDB + Elasticsearch quando o volume nao justifica.

---

## 5. Sem testes

"Vou adicionar testes depois." Depois nunca chega. Codigo sem testes e codigo que ninguem confia — nem voce. Refatoracao vira roleta russa. Deploy vira aposta. Cada mudanca pode quebrar algo sem ninguem saber.

**Correcao:** testes sao parte da feature, nao etapa separada. Funcionalidade sem teste nao esta entregue. CI que nao roda testes e CI decorativa.

---

## 6. Nao entender o que foi gerado

A IA gera 200 linhas de codigo. Voce da uma olhada rapida, parece bom, commita. Tres dias depois, algo quebra e voce nao sabe por que — porque nunca soube como funcionava.

**Correcao:** se voce nao consegue explicar o que cada parte do codigo faz, nao esta pronto para ser commitado. Peca para a IA explicar. Leia a documentacao das libs usadas. Entenda antes de aceitar.

---

## 7. Sem CI/CD

Codigo vai para producao via deploy manual. Sem lint, sem typecheck, sem testes automatizados no pipeline. Cada deploy e um ato de fe. Erros de tipagem passam, imports quebrados passam, testes falhando passam.

**Correcao:** GitHub Actions desde o primeiro commit. Lint + typecheck + testes + build. Se qualquer etapa falha, merge bloqueado. Sem excecao, sem bypass.

---

## 8. Tratar IA como magia

Acreditar que a IA vai "resolver tudo" se voce der o prompt certo. Isso leva a delegar decisoes arquiteturais para a IA, aceitar sugestoes sem questionar e perder o controle do sistema que voce esta construindo.

**Correcao:** IA e ferramenta, nao arquiteto. Ela executa bem o que voce define bem. Ela executa mal o que voce define mal. A qualidade do output e diretamente proporcional a qualidade do input — e o input e responsabilidade sua.
