# Materiais utilizados
- Learning Spark - Lightning-Fast Data Analytics
	- Página atual:  (Streaming)
	- [GitHub](https://github.com/databricks/LearningSparkV2)
- Ultimate Guide To Apache Spark Performance Tuning - Afaque Ahmad
	- Apache Spark Memory Management - 00:00
	- Apache Spark Executor Tuning | Executor Cores & Memory - 00:00


# Tópicos para aprofundar
Boas praticas de escrever código Spark, como estruturar o código

Catalyst Optimizer: Otimizador de consultas SQL do Apache Spark

Tungsten: Motor de execução física do Apache Spark, melhorar drasticamente o desempenho do Spark em três frentes:
- Uso eficiente de memória
- Processamento em CPU
- Geração de código otimizado (whole-stage code generation)

Otimizações das configurações do Spark


# Introdução
Apache **Spark é uma engine**, desenvolvida para o **processamento** de alto volume de dados distribuídos, com seus componentes sendo executados em um cluster. O Spark não funciona para o **armazenamento de dados**

Uma evolução ao seu antecessor, **Hadoop Map Reduce**, é na velocidade. O Spark conseguiu isso devido:
- Evita gravar dados em disco, mantendo tudo na RAM quando possível
	- Ele mantém os dados intermediários na memória RAM, sempre que possível. Isso elimina o custo de I/O em disco.
- Aproveita bem o hardware moderno
- Organiza as etapas como um DAG para distribuir bem as tarefas
	- Cada etapa de transformação (como filter, map, groupBy) vira um nó no grafo
	- O DAG scheduler do Spark analisa esse grafo e divide as tarefas para que possam ser executadas em paralelo nos nós do cluster


# Arquitetura
![[learning_spark_componentes_e_arquitetura_do_spark.png]]

Em alto nível, uma aplicação Spark consiste de um **Driver** responsável pela orquestração das operações que serão executas em paralelo no cluster

O driver acessa os componentes distribuídos no cluster, [[#Spark executor]] e [[#Cluster manager]], através do [[#Spark session]]

## Spark driver
Algumas de suas funções são as seguintes:
- Se comunicar com o **Cluster manager**
- Fazer a requisição de recursos (CPU, memoria, ...) a partir do **Cluster manager** para os **Spark’s executors**
- Transforma todas as operações do Spark em **DAGs**, fazer o schedule das DAGs e distribui suas execuções entre os **Spark’s executors**

Quando os recursos estão alocados, a comunicação é direta com os executores

## Spark session
O **Spark session**, a partir da versão 2.0, foi criado para centralizar tudo em um único ponto de entrada (Operações e dados), tornando o desenvolvimento mais simples

Ele substitui os objetos antigos como SQLContext, mas ainda é compatível com eles

## Cluster manager
The cluster manager is responsible for managing and allocating resources for the
cluster of nodes on which your Spark application runs.

Os seguintes métodos de deploy estão disponíveis:
- built-in standalone cluster manager
- Apache Hadoop
- YARN
- Apache Mesos
- Kubernetes

## Spark executor
A Spark executor, runs on each worker node in the cluster. The executors communi‐
cate with the driver program and are responsible for executing tasks on the workers

In most deployments modes, only a single executor runs per node

Cada executor roda dentro de uma **JVM**

## Spark job
Driver converte a aplicação em **jobs**. Cada um deles será transformado em uma DAG, que será o plano de execução do Spark

## Spark stage
Dentro do nó de cada **Job**, **Stages** são criadas para organizar quais tarefas serão executadas de maneira paralela e quais serão executadas de maneira sequencial

## Spark task
Uma **Stage** é formada por diversas **Tasks**, menor unidade de execução no Spark, que são distribuídas entre os executores do cluster

Cada **Task** é mapeada para um core e trabalha com uma partição dos dados

Ou seja, se um executor tem 16 núcleos, ele pode rodar 16 tasks em paralelo, cada uma cuidando de uma partição diferente

![[learning_spark_spark_tasks.png]]

## Distribuição dos dados e partições
Os dados são armazenados de maneira particionada dentro do cluster

O Spark, lida com cada partição como um DataFrame na memoria. Para haver otimização no gasto de rede no cluster, cada executor carrega a partição que está mais próxima de sua localização

Partições possibilitam um paralelismo mais eficiente. O Spark possui as seguintes estrategias de particionamento:
- [[#partitionBy]]
- [[#bucketBy]]

### Hash partitions
É uma forma de reorganizar os dados distribuídos com base em uma **função de hash aplicada à(s) coluna(s) de partição** com o objetivo de garantir que **linhas com o mesmo valor de chave** (ex: `user_id`) caiam **na mesma partição**.

Essa técnica é crucial para operações distribuídas eficientes, como: `joins`, `groupBy`, `aggregations`, ...


# Conceitos do Spark

## Spark query plan
![[spark_query_plan.png]]

São as etapas que o Apache Spark segue para transformar um código (em SQL ou DataFrame API) em tarefas físicas que serão executadas em um cluster. As etapas irão fazer as seguintes tarefas:

### 1. SQL AST / DataFrame API
É o código que você escreve:
- Ex: `df.select("nome").where("idade > 30")`
- Ou: `SELECT nome FROM pessoas WHERE idade > 30`

Essas instruções são transformadas em uma estrutura intermediária (AST – Abstract Syntax Tree).

### 2. Unresolved Logical Plan
É um plano lógico inicial, mas ainda com referências não resolvidas:
- O Spark ainda não sabe se as colunas e tabelas existem (ex: idade, pessoas)

É como escrever um plano sem verificar se os dados realmente estão lá

### 3. Analysis (com o Catalog)
Aqui o Spark consulta o catálogo (Hive Metastore, Delta Catalog, etc) para:
- Verificar nomes de tabelas
- Validar colunas e tipos
- Resolver nomes não qualificados

Saída será um **Logical Plan** resolvido

### 4. Logical Optimization
Spark aplica regras de otimização lógica:
- Predicate pushdown: move filtros para o início
- Constant folding: resolve expressões constantes antes
- Projection pruning: remove colunas não utilizadas

Saída será um **Optimized Logical Plan**

### 5. Physical Planning
Gera várias alternativas físicas para executar o plano:
- Join via broadcast?
- Join via sort-merge?
- Scan com parquet ou delta?

Saída será uma lista de **Physical Plans**

### 6. Cost Model
Spark usa um modelo de custo heurístico (como tamanho estimado de dados, cardinalidade, etc) para escolher a opção mais barata.

Saída será **Selected Physical Plan**

### 7. Code Generation (WholeStageCodegen)
Spark transforma o plano físico em código Java otimizado:
- Isso vira bytecode para ser executado no JVM
- Usa o conceito de RDDs internamente para paralelizar as tarefas

### 8. RDDs
Finalmente, o plano vira tarefas distribuídas executadas como RDDs:
- Cada etapa vira um Stage
- Cada Stage é dividido em Tasks que são distribuídos no cluster

### Adaptive Query Execution (AQE)
Recurso que **ajusta o plano físico de execução durante a execução da consulta**, com base em **estatísticas reais dos dados** (em vez de apenas estimativas feitas antes da execução). 

Melhorar a performance e eficiência das queries, adaptando o plano em tempo de execução. Está ativado por padrão no Spark 3.x

É executado após a etapa 7 e antes da etapa 8. Ele atua após a escolha inicial do plano físico, otimizando o plano com base em informações que só podem ser conhecidas durante a execução, como:
- Número de linhas lidas
- Tamanho de partições
- Skew nos dados

Com o AQE, o Spark pode:
1. Executar parte do plano
2. Coletar estatísticas reais (como número de linhas, tamanho de partições)
3. Reescrever partes do plano antes de continuar

O AQE pode atuar nas seguintes otimizações:
- Escolha Dinâmica de Join: Se um lado do join for muito pequeno, Spark pode decidir **fazer broadcast**
- Otimização de Skew: Detecta partições desbalanceadas (skew) e divide partições grandes e também rebalanceia os dados
- Reparticionamento Dinâmico: Reduz ou aumenta o número de shuffle partitions com base no volume real de dados, o que evita pequenas partições inúteis e partições grandes demais que causam OOM

Pense no **AQE como um "replanejador de rota" no Waze**. Você definiu a rota antes da viagem (Selected Physical Plan),  mas durante o trajeto (execução), ele percebe um problema e muda a rota (Adaptive Physical Plan).

**Adaptive Physical Plan** é a etapa onde ocorre o **AQE** e é executada após a etapa de **Selected Physical Plan**

## Gerenciamento de memoria no Spark

## Shuffle
É o processo de redistribuir dados entre diferentes partições, de forma que tarefas subsequentes possam operar com os dados corretamente agrupados ou ordenados

Ele normalmente acontece quando uma operação requer que dados que estavam em diferentes nós do cluster sejam reorganizados, **wide operations**. Por exemplo, operações que dependem de chaves iguais estarem no mesmo nó

Essa troca intensa de dados é caro em termos de rede e disco, otimizá-lo é essencial. O shuffle é problemático pelos seguintes motivos:
- Custo de rede alto: dados são transferidos entre máquinas
- Escrita e leitura em disco: afeta a performance
- Uso de memória: os dados precisam ser reagrupados e armazenados temporariamente

No **shuffle**, acontecem as seguintes tarefas:
- Escrita em disco: Cada executor escreve os dados particionados em disco
- Movimentação de dados pela rede: As partições são transferidas para outros executores (network I/O)
- Leitura dos dados: O executor de destino lê os dados recebidos e os processa

O **shuffle** acontece quando uma das seguintes operações é executada:
- `groupBy()`
- `join()`
- `reduceByKey()`
- `distinct()`
- `repartition()`
- `sortBy()`

Uma analogia para o **shuffle** é como misturar cartas de baralho entre jogadores para reorganizá-las por naipe. Cada jogador (executor) precisa enviar e receber cartas (dados), e isso leva tempo, memória e rede

## Transformations, Actions e Lazy Evaluation
As operações realizadas nos das dados, podem ser classificadas em duas categorias:
- Transformations
- Actions

**Transformations** irão realizar transformações em um DataFrame (filter, rename, ...) e o resultado será um novo DataFrame sem alterar o DataFrame de origem, os DataFrames são imutáveis

Porém, as transformações não são executadas/processadas imediatamente, o Spark faz um planejamento de execução e quando uma operação do tipo **Action** é solicitada, ela aciona  esse planejamento

Essa estrategia de adiar a execução até que os dados sejam "tocados" (leitura ou escrita no disco), é chamada de **Lazy Evaluation**. Essa estrategia permite que o Spark rearrange o planejamento de execução e consiga otimizá-lo para chegar no planejamento mais eficiente

Abaixo está um diagrama com **Transformations** e **Action**, que ativa o **Lazy Evaluation**. Cada **Transformations** gera um novo DataFrame

![[learning_spark_lazy_evaluation.png]]

**Transformations** podem ser classificadas em dois tipos:
- **Narrow operations:** Operações que não requerem **shuffle**. Operações como:
	- `filter()`
	- `withColumn()`
	- `select()`
- **Wide operations:** Operações que requerem **shuffle**. Operações como:
	- `joins()`
	- `groupBy()`

Toda **Action** cria um novo **job**

## Structured APIs
Quando utilizamos as Structured APIs, estamos falando para o Spark "o que deve ser feito" e não "como deve ser feito"

Isso porque o **Spark SQL engine** será responsável para gerar a query mais performática

## Data Spill
**Data Spill** ocorre quando o Spark **não consegue manter todos os dados de uma operação intermediária (como `join`, `sort`, `groupBy`, etc.) inteiramente na memória RAM**. Quando isso acontece, o Spark escreve parte desses dados **temporariamente no disco** para continuar o processamento.

Se o executor **não tiver memória suficiente**, o Spark **"spilla"** os dados para disco (SSD/HDD), o que é **muito mais lento**.

Efeitos problemáticos:
- **Reduz o desempenho drasticamente** (acesso ao disco é muito mais lento que à RAM)
- Pode gerar **uso intenso de disco** e gargalos de I/O
- Se o disco encher, pode até causar falhas no job

Existem os seguintes tipos de **spill:**
- Shuffle spill: Durante shuffle operations (e.g. groupByKey)
- Sort spill: Durante ordenações que não cabem na memória
- Aggregation spill: Durante reduceByKey, groupBy, agg, etc.
- Join spill: Em joins grandes, principalmente sort-merge join

## Data Skew
Problema gerado quando as partições estão desbalanceadas, o que faz com que uma partição leve muito mais tempo para ser processada do que as outras

Podemos identificar das seguintes maneiras:
- **Event timeline:** Ver se alguma partição levou muito mais tempo para ser processada
- **Summary metrics:** Ver se o tempo máximo foi muito maior que o tempo médio

As seguinte operações podem causar **data skew:**
- `groupBy()`
- `join()`

Os impactos desse problema são:
- Longo tempo para finalização dos jobs
- Não utilização da capacidade maxima do cluster, idle cores
- Out Of Memory errors

Podemos lidar com esse problema através das seguintes estrategias:
- [[#Adaptive Query Execution (AQE)]]
- [[#Broadcast Hash Join]]
- [[#Salting]]


# Boas práticas

## Definição manual do schema
Definição manual do schema proporciona os seguintes benefícios:
- Tira o trabalho do Spark de inferir o schema
- Previne a criação de um job para apenas ler uma grande porção dos dados e inferir o schema
- Erros são detectados rapidamente se os dados não se encaixarem no esquema desejado

Utilizando `samplingRatio` o Spark pode inferir o schema a partir de uma amostra menor de dados

Arquivos **parquet** já salvam o schema nos metadados


# Otimizações

## Particionamento dos dados
[[armazenamento_de_dados#Particionamento dos dados]]

Podemos definir o tamanho das partições que o Spark irá ler com o comando: `spark.sql.files.maxPartitionBytes`

Algumas estrategias de particionamento dos dados no Spark são:

### partitionBy
Os dados serão separados em diversas pastas de acordo com as colunas utilizadas no particionamento. Se iremos particionar por mais de uma coluna, as pastas serão criadas pela ordem das colunas

`partitionBy` é ideal nas seguintes situações:
- Filtros frequentes em queries: Reduz o scan de dados
- Grandes volumes de dados: O particionamento melhora a leitura e permite processamento paralelo eficiente
- Incrementos por chave de negócio: Se você recebe dados incrementais por uma chave específica (ex: data_ingestao, loja_id), particionar por essa chave evita regravar tudo

`partitionBy` não é ideal nas seguintes situações:
- Particionar por colunas com alta cardinalidade: [[#Small file problem]]
- Se o dataset é pequeno e não há ganho significativo
- Se as queries raramente filtram por essa coluna

### bucketBy
Os dados serão separados em um número determinado de buckets de acordo com o hash de uma ou mais colunas. Diferente de `partitionBy`, não cria uma pasta por valor, e sim um número fixo de arquivos (`numBuckets`)

Para definir o número ótimo de buckets, podemos seguir os passos abaixo:
- Pegar o tamanho do dataset (MB)
- O número de cada bucket é de 128 até 200 MB
- Achamos o número de buckets através do calculo: tamanho do dataset / 128 ~ 200

`bucketBy` é ideal nas seguintes situações:
- Colunas usadas em **joins frequentes**: Melhora performance evitando shuffle (quando tabelas têm mesmo `bucketBy` e número de buckets)
- Dados com alta cardinalidade, onde `partitionBy` seria ineficiente
- Cenários em que o número de buckets é fixo e definido previamente

`bucketBy` não é ideal nas seguintes situações:
- Os dados são muito dinâmicos e precisam de escrita incremental frequente
- Não existe coluna natural de junção ou filtro que seja estável

## Shuffle partition Spark optimization
Shuffle partition são as partições geradas após o shuffle
 
Por default o  número de **shuffle partitions**, `spark.sql.shuffle.partitions`, é 200. Então se o cluster possuir 1000 cores, teremos o seguinte cenário:
- Working cores: 200
- Idle cores: 800

Isso gera dois problemas:
- Longo tempo para finalização dos jobs
- Não utilização da capacidade maxima do cluster

O ideal é que cada **shuffle partition**, tenha um tamanho de 100 ~ 200 MB

Para otimizar o número ideal de **shuffle partitions**, devemos entender como são os dados

Para um **grande volume de dados por shuffle partition**, podemos fazer o seguinte calculo:
- tamanho do dataset (bytes) / quantidade de bytes que idealmente cada partição deve receber

Para um **baixo volume de dados por shuffle partition**, podemos fazer o seguinte calculo:
- tamanho do dataset (bytes) / quantidade de bytes que idealmente cada partição deve receber

Para esse caso também podemos definir o tamanho da **shuffle partition** ideal, pelo calculo:
- tamanho do dataset (bytes) / número de cores

Caso a performance do job ainda esteja ruim após a definição do número de **shuffle partition**, os dados podem estar com o problema de [[#Data Skew]]

## Caching dos dados
No Spark, possuímos duas funções para essa tarefa, **cache** e **persist**

Ambos possuem o mesmo objetivo, de melhorar a performance no acesso de DataFrames que são frequentemente acessados. Uma diferença, é que o método **persist** oferece mais controle em como e onde os dados serão salvos

Vale destacar que os dados não são armazenados imediatamente, que nem o **Lazy Evaluation**, os dados são salvos apos uma **action**

Como regra geral, a utilização do cache em memoria deve ser utilizado de maneira criteriosa, uma vez que pode implicar em custos de recursos na serialização e desserialização, dependendo do StorageLevel utilizado

É **indicado** utilizar essas estrategias, quando queremos acessar um grande dataset diversas vezes em queries e transformações, exemplos:
- DataFrames acessado diversas vezes em treinamento de ML
- DataFrame acessado diversas vezes para realizar transformações em pipelines de ETL

Porém, nem todos os cenários são indicados na utilização dessas estrategias, abaixo estão  exemplos onde elas **não são indicadas**:
- DataFrame que é muito grande e não cabe na memoria
- Uma transformação barata em um DataFrame que não requer uso frequente, independentemente do tamanho

### DataFrame.cache()
De acordo com o total de memoria disponível entre os [[#Spark executor]], o maior número de partições serão salvas. Vale destacar que somente partições completas podem ser salvas. 

**Exemplo:** Se um DataFrame possui 8 partições e 4.5 partições cabem na memoria, somente 4 partições serão salvas

Vale destacar que, as partições que não forem salvas, terão que ser reprocessadas

### DataFrame.persist()
Possui o mesmo objetivo do **cache**, porém, disponibiliza maior controle sobre como e onde os dados serão salvos. Os métodos disponíveis para salvar os dados são:
- MEMORY_ONLY
- MEMORY_ONLY_SER
- MEMORY_AND_DISK
- DISK_ONLY
- OFF_HEAP
- MEMORY_AND_DISK_SER

![[ultimate_guide_to_apache_spark_performance_tuning_storage_level_types.png]]

## Spark joins
O Spark tenta usar estratégias de join eficientes para minimizar o custo com [[#Shuffle]], essas são:
- broadcast hash join (BHJ)
- shuffle hash join (SHJ)
- shuffle sort merge join (SMJ)
- broadcast nested loop join (BNLJ)
- huffle-and-replicated nested loop join (a.k.a. Cartesian product join)

As estratégias mais comunssão: Broadcast Hash Join e o Sort Merge Join

### Broadcast Hash Join
Também conhecido como map-side-only join

O **Broadcast Hash Join** é uma estratégia de **join eficiente** usada pelo Spark quando um dos DataFrames (ou tabelas) é **pequeno o suficiente para caber na memória do driver e de todos os executores** do cluster

Ele funciona da seguinte maneira:
- O Spark detecta que um dos lados do join é pequeno (por padrão, < 10 MB mas pode ser configurado)
-  Ele distribui (broadcast) esse pequeno DataFrame para todos os nós do cluster
- Cada executor mantém o DataFrame pequeno em memória e faz o join localmente com sua própria partição do DataFrame maior
- Não há shuffle de dados do DataFrame grande, só o pequeno é replicado

![[learning_spark_broadcast_hash_join.png]]

Essa estratégia possui as seguintes vantagens:
- Rápido e eficiente: elimina shuffle no lado maior
- Economiza custo computacional e reduz tempo de execução

Porém, essa estratégia só funciona bem se o DataFrame pequeno realmente for pequeno. Se ele for grande, pode causar **OutOfMemoryError** nos executores

Para o máximo beneficio deste join, devemos utilizá-lo mediante as seguintes condições:
- Quando cada chave dentro do data set pequeno e o maior estão hashed para a mesma partição. A fim de evitar o **shuffle**, pois cada executor pode fazer o join localmente
- Um DataFrame é muito menor que o outro e o DataFrame atende aos limites de memoria (por padrão, < 10 MB mas pode ser configurado)
- O join é do tipo **Equi-join**. BHJ só funciona para joins com igualdade `=`, não funciona para joins com condições como `>` ou `!=`
- Rede e memória não são um problema. Como o Spark replica o DataFrame pequeno em todos os executores, isso consome banda de rede e memória

Ponto importante, se o broadcast automático estiver desativado `spark.sql.autoBroadcastJoinThreshold = -1`

O Spark nunca fará broadcast, e vai usar o Shuffle Sort Merge Join (SMJ) no lugar

### Shuffle Sort Merge Join
É uma estratégia de join usada para unir dois grandes conjuntos de dados com base em uma chave comum. Esta chave deve ser:
- Ordenável
- Única
- E com valor que possa ser usado para particionar os dados

O Spark precisa garantir que todas as linhas com o mesmo valor de chave estejam na mesma partição e, idealmente, no mesmo executor. Essa estratégia ocorre na seguinte ordem:
1. Shuffle:
	- O Spark redistribui (embaralha) os dados de ambos os DataFrames com base na chave de junção. O objetivo é garantir que todas as linhas com a mesma chave fiquem na mesma partição, mesmo que venham de nós diferentes do cluster. Esse é o passo mais custoso em termos de rede e performance.
2. Sort (por partição):
	- Depois do shuffle, **cada partição é ordenada localmente** pela chave de junção.
3. Merge:
	- O Spark faz a junção dos registros em cada partição, comparando as chaves ordenadas e combinando as que são iguais

Com a estrategia de **buckets** ([[#bucketBy]]), podemos evitar o **shuffle** para os joins de igualdade (equi-joins)

Para o máximo beneficio deste join, devemos utilizá-lo mediante as seguintes condições:
- Quando cada chave em dois grandes conjuntos de dados pode ser ordenada e particionada para a mesma partição pelo Spark
- Quando você deseja realizar apenas _equi-joins_ para combinar dois conjuntos de dados com base em chaves ordenadas e correspondentes
- Quando você deseja evitar operações de _Exchange_ e _Sort_ para economizar grandes _shuffles_ na rede

## Salting
Técnica para que visa solucionar problema de [[#Data Skew]]

Essa técnica consiste em adicionar valores aleatórios nas colunas utilizas para fazer o `join` e `groupby`. Desta maneira, os valores desbalanceados são distribuídos de uma maneira mais uniforme, resolvendo o problema de **data skew**

Devemos escolher bem o número de salt, para chegar em um número ideal de partições

### join
No dataframe com **skew**, criamos uma coluna com os valores de salt

No outro dataframe, preferencialmente o menor, criamos uma coluna que será um array com todos os valores possíveis do salt e depois fazemos um explode. Para as chaves terem todas as combinações possíveis

**Observação:** explode é custoso e aumenta o dataframe

Depois, fazemos o `join` pela coluna de chave e salt

### group by
Criamos uma coluna com os valores de salt

Fazendo duas operações de group by:
- Primeiro, agrupamos pelas chaves e a coluna de salt
- Para ter o resultado final, agrupamos somente pelas chaves
