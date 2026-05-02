
**Data do arquivamento:** 11/08/2025

Softwares com o objetivo de armazenar dados, são divididos em duas categorias principais:
- Relacionais: Armazenar dados estruturados
- NoSQL: Armazenar dados semi e não estruturados

NoSQL está relacionado com cluster, computação distribuída ([[Computação distribuída - Hub]])

# Informações da aula
* https://linktr.ee/devtbd
* https://db-engines.com/en/ranking
* https://cloud.mongodb.com/v2/66343640bd8774654a8ccf47#/overview
* andreatti / cde34RFV 
* mongodb+srv://andreatti:cde34RFV@cluster0.erzkij8.mongodb.net/

# Índices

A medida que o banco de dados cresce, vai ficando mais difícil realizar operações de manipulação e recuperação de dados de forma otimizada. 

Índices é uma estrutura de dados que acelera a recuperação de dados sem a necessidade de verificar cada registro.

Índices devem ser utilizados com sabedoria, pois utilizar eles de forma mal planejada podem causar gargalos nas operações de escrita, principais gargalos em banco de dados:
- Falta de índices
- Índices mal projetados

Determinar índices é um equilíbrio entre respostas rápidas às consultas e custos de atualização.

Índice não melhoram a escrita ou tamanho de armazenamento dos dados.

Estrutura de dados utilizadas para implementação de índices:
- Árvore B
	- Árvore balanceada
	- Mantém dados ordenados
	- Permite pesquisas, acesso sequencial, inserção e exclusão em tempo logarítmico
- Árvore B+
## Estrutura

Índices são compostos por lista ornada de valores, com cada valor conectado a ponteiros que levam às páginas de dados onde esses valores estão persistidos.

**Como é a estrutura de um índice? Pesquisar**

## Índices agrupados x não agrupados

### Agrupados (Clustered Index)

Reordenam a maneira como os registros são armazenados fisicamente.

Os registros não são armazenados de maneira aleatória ou na ordem que foram inseridos, estes são organizados para se alinharem com a ordem do índice, daí o termo agrupado. Os atributos específicos usados para organizar essa ordem são chamados de chaves clusterizada.

Se este índice for aplicado para uma coluna de idade, os registros serão gravados com inicio pelo registro mais novo até o mais velho.

### Não Agrupados (Non-clustered Index)

Ordem física dos dados não é igual à ordem lógica estabelecida pelo índice.

Parei no minuto 48:32

## Conjectura RUM

Estruturas de Dados, Arvore B e B+

[[modelagem_de_dados]]