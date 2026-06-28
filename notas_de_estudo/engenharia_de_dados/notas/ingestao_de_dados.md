Esta nota nota é um tópico da nota [[engenharia_de_dados]] e tem como objetivo ser o hub para os temas de **ingestão de dados**


# Materiais utilizados
- Fundamentals of Data Engineering: Plan and Build Robust Data Systems
- Data Engineering Design Patterns: Recipes for Solving the Most Common Data Engineering Problems
- [Backfilling Historical Data With Idempotent Data Pipelines](https://www.ml4devs.com/what-is/backfilling-data/)
- [Backfilling Data Guide: The Ultimate Manual for 2024](https://atlan.com/backfilling-data-guide/)


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


### Backfill
É o reprocessamento histórico de dados, com o objetivo de fazer alguma correção ou ajuste dos dados. Como por exemplo, atualizar uma coluna devido a uma alteração na lógica do negócio. Pode ser realizado em processos **Batch** ou **Streaming**.

#### Melhores práticas
- Definição de escopo: A fim de minimizar o processamento, definir quais tabelas, partições e intervalo terão que ser reprocessados. As pipelines devem aceitar parâmetros como start_date e end_date
- Idempotência: Rodar 1x ou 10x gera o mesmo resultado final 
- Isolamento e versionamento: Ter ambiente de testes, para validar os dados antes de atualizar as tabelas produtivas
- Ingestão incremental: Fazer reprocessamento do registro mais antigo para o mais novo em batches, para não sobrecarregar nenhum sistema
- Observabilidade: Fazer log de todo o processo (parâmetros, status, ...), a fim de acompanhar a execução
- Validação: Validar resultado antes de atualizar tabelas produtivas
- Comunicação: Comunicar todas as partes interessadas, sobre o andamento do reprocessamento


#### Passo a passo
1. Preparação
	- Definir quais dados/partições irão passar pelo processo de backfill
	- Ter backup para caso de algum problema
2. Transformação dos dados
	- Além da correção/ajuste que será executado, garantir que os dados estejam no schema esperado
3. Implementação
	- Para altos volumes, realizar a ingestão de maneira incremental e em batches. Assim, conseguimos ter uma melhor observabilidade, ter pontos de controle para eventuais falhas e evitar sobre carga nos sistemas
4. Validação
	- Antes de atualizar as tabelas produtivas, fazer as validações e testes no ambiente de testes/homologação
	- Observabilidade, devemos conseguir acompanhar o status de execução de todo o processo. Habilitar logs para todas as etapas
5. Gerenciamento de erros
	- Ter processo bem definido de rollback, caso seja necessário
6. Documentação
	- Documentar a causa (correção ou ajuste), fontes de dados, transformações, testes, validações. Essa documentação será referência para o futuro
7. Comunicação
	- Alinhar com todas as partes interessadas as alterações que serão aplicadas, quando serão aplicadas, planejamento definido e os status durante a execução


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
