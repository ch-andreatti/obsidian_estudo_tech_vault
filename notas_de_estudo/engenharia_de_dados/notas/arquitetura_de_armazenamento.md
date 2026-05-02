# Data Warehouse
Since the last century, databases and SQL queries have been known as great building
solutions for BI workloads. However, the last decade has seen two major new trends
in analytical workloads:

Growth in data sizes
With the advent of big data, there has been a global trend in the industry to
measure and collect everything (page views, clicks, etc.) in order to understand
trends and user behaviors. As a result, the amount of data collected by any com‐
pany or organization has increased from gigabytes a couple of decades ago to ter‐
abytes and petabytes today.

Growth in the diversity of analytics
Along with the increase in data collection, there is a need for deeper insights.
This has led to an explosive growth of complex analytics like machine learning
and deep learning.

Databases have been shown to be rather inadequate at accommodating these new
trends, because of the following limitations:

Databases are extremely expensive to scale out
Although databases are extremely efficient at processing data on a single
machine, the rate of growth of data volumes has far outpaced the growth in per‐
formance capabilities of a single machine. The only way forward for processing
engines is to scale out—that is, use multiple machines to process data in parallel.
However, most databases, especially the open source ones, are not designed for
scaling out to perform distributed processing. The few industrial database solu‐
tions that can remotely keep up with the processing requirements tend to be pro‐
prietary solutions running on specialized hardware, and are therefore very
expensive to acquire and maintain.

Databases do not support non–SQL based analytics very well
Databases store data in complex (often proprietary) formats that are typically
highly optimized for only that database’s SQL processing engine to read. This
means other processing tools, like machine learning and deep learning systems,
cannot efficiently access the data (except by inefficiently reading all the data from
the database). Nor can databases be easily extended to perform non–SQL based
analytics like machine learning.
These limitations of databases led to the development of a completely different
approach to storing data, known as data lakes.

# Data Lake

**Data lake** é uma arquitetura de armazenamento de dados que possibilita o armazenamento de:
- Alto volume de dados;
- Arquivos em diferentes formatos ([[formato_de_arquivos_para_engenharia_de_dados]]) 

Com essa arquitetura não é necessário realizar o pre-processamento para armazenar os dados, eles podem ser armazenados em seu formato original.

Esta arquitetura é ideal para casos onde se possui dados com alto volume, variedade ou velocidade.

Deve ser estabelecido uma boa [[governanca_de_dados]] para o **Data Lake**, para que ele não vire um **Data Swamp**.

Normalmente, o **Data Lake** é dividido em camas com objetivos específicos, essas camadas são:
- **Raw Zone:** Zona de chegada do dados, nesta camada não há transformação/manipulação dos dados, eles são empilhados em seu formato original, nesta camada pode haver dados duplicados;
- **Process Zone:** Nesta etapa os dados são processados para serem consumidos e extraídos de maneira confiável na próxima etapa. Nesta camada normalmente os dados ficam organizados de maneira generalizada;
- **Access Zone:** Nesta etapa são aplicadas as regras de negócio, geralmente os dados desta etapa servem para resolver algum problema de negócio ou aplicação.

Data lakes are not without their share of flaws, the most egregious of which is the lack
of transactional guarantees. Specifically, data lakes fail to provide ACID guarantees

Porém, **data lakes não estão isentos de falhas**, sendo a mais grave a ausência de garantias transacionais. Especificamente, **data lakes não conseguem oferecer garantias ACID**
# Lakehouse
Arquitetura que combina os pontos positivos dos **Data Warehouse** e **Data Lake** para OLAP workloads. Lakehouses são viabilizados por um novo tipo de arquitetura que oferece funcionalidades de gerenciamento de dados similares às de bancos de dados, diretamente sobre o armazenamento barato e escalável utilizado em data lakes.

Algumas de suas principais características são:
- Transaction support: Suporte para [[acid_transactions]]
- Schema enforcement and governance: Verificação e possibilita evolução do schema
- Support for diverse data types in open formats
- Support for diverse workloads
- Support for upserts and deletes: Possibilita casos complexos de atualização como **change-data-capture (CDC)** e **slowly changing dimension (SCD)**
- Data governance

Conseguimos desenvolver **Lakehouses** através das seguintes tecnologias:
- Apache Hudi
- Apache Iceberg
- Delta Lake

Em um nível bem alto, todos os três projetos têm uma arquitetura semelhante, inspirada em princípios bem conhecidos de bancos de dados. Eles são todos formatos abertos de armazenamento de dados que fazem o seguinte:
- Armazenam grandes volumes de dados em formatos de arquivos estruturados em sistemas de arquivos escaláveis
- Mantêm um log de transações para registrar uma linha do tempo de mudanças atômicas nos dados (semelhante aos bancos de dados)
- Usam esse log para definir versões dos dados da tabela e fornecer garantias de isolamento por snapshot entre leitores e escritores

