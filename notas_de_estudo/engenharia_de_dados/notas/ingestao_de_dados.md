Esta nota nota é um tópico da nota [[engenharia_de_dados]] e tem como objetivo ser o hub para os temas de **ingestão de dados**


# Materiais utilizados
- Fundamentals of Data Engineering: Plan and Build Robust Data Systems
- Data Engineering Design Patterns: Recipes for Solving the Most Common Data Engineering Problems


# Geral

## Questionamentos
Antes de escolher como ingerir dados, é importante responder perguntas como:
- Em qual **formato** os dados estão?
- Os sistemas **downstream** (armazenamento, transformação) conseguem lidar com esse formato?
- O dado está pronto para uso imediato?
- Se for **streaming**, precisa ser transformado antes de chegar ao destino?  
- É viável transformar durante o fluxo?

## Tipos de ingestão

### Batch
- É uma forma de lidar com dados contínuos em **"lotes" periódicos** (ex: 1 vez por dia).
- Os lotes são criados com base em um **intervalo de tempo** ou quando o dado atinge um **tamanho específico**.
- O modelo **batch é unidirecional**: uma vez que o dado vira lote, há uma latência fixa até ser processado.
- Muito usado por causa das **limitações de sistemas legados**.
- Continua sendo **popular para análises e machine learning**, onde a latência não é tão crítica.

### Streaming
- Quase todos os dados são, por natureza, **produzidos continuamente**.
- Ingestão em streaming permite o **envio contínuo em tempo real** para outros sistemas (apps, bancos, ferramentas analíticas).
- **Tempo real** significa que os dados chegam ao destino **pouco tempo após serem gerados** (ex: < 1 segundo).
- A **latência aceitável** depende do contexto e das necessidades do sistema.


# Design patterns
Abaixo estarão descritos os padrões utilizados para ingestão, para mais informações ver a nota [[data_engineering_design_patterns]]

## Full load
Também conhecido como overwrite, consiste na leitura e escrita completa dos dados, recriamos a tabela do zero

Mediante as seguintes situações, esse **dp** pode ser uma boa escolha:
- Os dados são atualizados poucas vezes
- Não temos informações, sobre quais foram os últimos dados ingeridos e atualizados
- O sistema de origem realiza hard deletes ([[engenharia_de_dados#Tipos de delete]])

Apesar de ser um abordagem simples e intuitiva, esse **dp** possui algumas consequências:
- **Volume dos dados:** Os custos (tempo e financeiramente) irão crescer conforme o crescimento dos dados, dificultando a escalabilidade
- **Consistência dos dados:** Não possuímos histórico, apenas os dados mais atualizados, isso pode ser um problema dependendo da necessidade

## Incremental load
Ingestão incremental dos dados, onde apenas os novos e os dados que foram alterados são ingeridos em determinada frequência (diariamente, semanalmente, ...)

Este **dp** é uma boa abordagem quando, estamos lidando com dados que crescem ao longo do tempo e este possui características para identificar os novos dados e os que foram alterados. Podemos fazer a implementação de duas maneiras:
- Utilizando **delta columns**, colunas que identificam os novos dados desde última execução
- Utilizando **partições de tempo**, similar na XP, separamos os dados em partições de tempo e pegamos apenas as partições novas

Apesar de ser um abordagem performática, esse **dp** possui algumas consequências:
- **Hard deletes:** Não conseguimos pegar esse tipo de delete, fazendo nossos dados ficarão diferentes da fonte

## Data readiness
Este **dp** tem como objetivo fazer com que a ingestão dos dados ocorra no momento correto, após os dados upstream terem sidos atualizados

### Problema
Possuímos diversas pipelines, gerenciadas por diferentes times e não sabemos quando devemos executar as nossas pipelines. Como precisamos de dados que não gerenciamos, não sabemos quando eles foram atualizados para iniciar as nossas pipelines

### Solução
Podemos adotar duas implementações

#### Flag file
Salvar um arquivo no blob, indicando a última vez que os dados foram atualizados com sucesso. Com essas flags, as pipelines **downstream** podem utilizar esses arquivos para decidir quando devem serem iniciadas

**Essa abordagem é utilizada na XP**

#### Partição
Fazer o controle através das partições, porém, irá funcionar quando os dados crescerem com o tempo
