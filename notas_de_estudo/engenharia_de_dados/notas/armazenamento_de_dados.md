Esta nota é um tópico da nota [[engenharia_de_dados]] e tem como objetivo ser o hub para os temas de **armazenamento de dados**.

Está é uma etapa critica do ciclo da [[engenharia_de_dados]], pois ela abrange diversas estágios, como:
- Ingestão
- Transformação
- Disponibilização


# Materiais utilizados
- Fundamentals of Data Engineering: Plan and Build Robust Data Systems
- Data Engineering Design Patterns: Recipes for Solving the Most Common Data Engineering Problems
- [Data partitioning guidance](https://learn.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning)


# Geral

## Tipos de delete
- **Hard delete:** Os dados são apagados fisicamente, não sendo possível a recuperação após essa operação
- **Soft delete:** Os dados são marcados (flag, timestamp, ...) como deletados, mas não são apagados fisicamente, possibilitando a recuperação dos dados


## Small files problem
É a situação onde possuímos **muitos arquivos pequenos** em vez de poucos arquivos grandes. Em vez de **10 arquivos de 1GB** possuímos **1.000.000 arquivos de 10KB**, mesmo volume lógico, mas com os seguintes problemas:
- **Sobrecarga de metadata** 
	- Antes de ler os dados, é preciso: 
		- Listar arquivos
		- Abrir conexões
		- Ler metadados (schema, tamanho, etc)
	- Em alguns casos, a tarefa de **listar arquivos pode ser mais lento que processar os dados**
- **Explosão de tarefas (tasks)**
	- Cada arquivo pequeno pode virar 1 task, resultando:
		- Milhões de tasks
		- Overhead de scheduling. CPU perdida com orquestração, não com dados
- **Uso ineficiente de I/O, rede e custos**
	- Mais chamadas de rede (abrir, fechar e listar arquivos)
	- Menos throughput
	- Pior cache
	- Mais operações de `LIST`, `GET`, `HEAD` nos sistemas de armazenamento, aumentando os custos

Este problema não é relacionado aos dados, é relacionado ao excesso de arquivos, tasks e metadados

### Causas comuns
- Streaming mal configurado: Spark escrevendo micro-batches
- Partições demais: `partitionBy(year, month, day, hour, minute)`
- Paralelismo alto: 10.000 executors escrevendo
- CDC / eventos: 1 arquivo por evento
- Reprocessamentos: Append sem compaction


### Resolução
Para resolver esse problema, o foco principal é controlar o número e o tamanho das partições ao salvar os dados físicos

Para formatos colunares (Parquet/Delta/Iceberg), o tamanho ideal de cada arquivo fica em torno de **128MB ~ 1GB**. Menor que **10MB** quase sempre problema e Menor que **1MB** costuma dar problema grave


# Escolha do sistema ideal para armazenamento dos dados
Quando estamos escolhendo um sistema para armazenamento, devemos verificar os seguintes pontos:
- **Scalability and performance:** A solução de armazenamento deveria ser capaz de escalar conforme o volume de dados e disponibilizar o **throughput** necessário para leitura/escrita e a **latência** requiridas pelos workloads
- T**ransaction support:** Diversos workloads estarão lendo e escrevendo dados de maneira concorrente, o ideal é que o sistema possua suporte para [[acid_transactions]], a fim de garantir a qualidade dos dados
- **Support for diverse data formats:** A solução de armazenamento deveria ser capaz de armazenar dados estruturados, semi estruturados e não estruturados
- **Support for diverse workloads:**
	- SQL workloads (BI)
	- Batch workloads (ETL)
	- Streaming workloads
	- ML and AI (Como predições)
- **Openness:** Como diferentes workloads têm requisitos diferentes (batch, streaming, ML), é essencial armazenar os dados em formatos **abertos e interoperáveis** (parquet, ORC, avro, ...). Assim, qualquer engine (Spark, Trino, Flink, etc.) pode ler/escrever os dados sem dependência de um fornecedor específico.

**Latência:** É o tempo que demora para começar e completar uma operação.
**Throughput:** É a quantidade de dados processados por segundo (ou operações por segundo).

Aqui vale entender o tipo de workload que estamos trabalhando:
- OLTP: Muitas requisições com baixa latência
- OLAP: Poucas requisições com alto throughput

Outro ponto que podemos olhar para uma solução de armazenamento, é como ela lido com a **temperatura dos dados** (frequência de acesso):
- **Hot data:** Acessados com **muita frequência**, até várias vezes por segundo. Devem ser armazenados de forma que o acesso seja **rápido**, de acordo com a necessidade do caso de uso.
- **Lukewarm data:** Acessados **de vez em quando**, como semanalmente ou mensalmente.
- **Cold data:** Raramente acessados, armazenados principalmente para **compliance** ou em casos de **falha catastrófica**. Na nuvem, há **tiers de armazenamento específicos**, com **baixo custo mensal**, mas **alto custo de recuperação**.

Esse conceito ajuda a **otimizar custo e desempenho**, escolhendo o tipo de armazenamento mais adequado para cada tipo de dado.

O livro **Fundamentals of Data Engineer** tem outros questionamentos relevantes para se levantar antes de escolher uma solução para o armazenamento


# Técnicas de otimização
Além de escolher o sistema ideal de armazenamento de dados, devemos garantir que ele será utilizado da maneira mais otimizada, pois a estrategia muda conforme a cardinalidade da coluna:
- Baixa e média: Particionamento
- Alta: Bucketing e ordenação

## Particionamento
Uma analogia para o particionamento, seria em quebrar um grande bloco em bloco menores (partições), através por determinados atributos (colunas)

É fundamental entender os dados antes de escolher as colunas que serão utilizadas para fazer o particionamento, pois a estrategia muda dependendo da cardinalidade da coluna:
- Baixa ou média: Particionamento
- Alta: Bucketing e ordenação

**Cardinalidade** é a quantidade de valores distintos que uma coluna possui

Um ponto de partida para escolher as colunas de particionamento, é em utilizar as colunas que serão utilizadas em filtros (`where`) e agrupamentos (`groupBy`). Não é ideal, escolher colunas que possuem valores que são frequentemente alterados

Os benefícios de utilizar partições são: 
- Paralelismo mais eficiente
- Acesso mais rápido dos dados

**Ponto importante:** Devemos analisar se as participações estão sendo divididas corretamente ou se tivemos **skew** em algumas delas,  que irá comprometer os ganhos de performance

As maneiras mais comuns de se realizar o particionamento são:
- Particionamento horizontal / Sharding: Todas as partições irão possuir o mesmo schema
- Particionamento vertical: Cada partição terá um subset das colunas


## Indices
(Nota em desenvolvimento)


# Design patterns
Abaixo estarão descritos os padrões utilizados para ingestão, para mais informações ver a nota [[data_engineering_design_patterns]]

## Data compaction
Este **dp** tem como objetivo reduzir o **footprint** dos dados, ou seja, diminuir o espaço físico em disco que os dados ocupam. Ou seja, o foco dele é em combate o problema de [[armazenamento_de_dados#Small files problem]]

Ponto importante, esse **dp** é mais seguro e simples de ser implementado utilizando tecnologias que conseguem garantir [[acid_transactions]]
