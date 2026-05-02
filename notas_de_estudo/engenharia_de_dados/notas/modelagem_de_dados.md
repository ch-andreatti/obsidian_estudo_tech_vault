# Notas soltas
Estudo modelagem

28/05 OK Relacional

Modela transacional, representa o mundo ideal (Relacional)
Objetivo de registrar operação da empresa

Conceitual - Neogio, definir relaçãoes
Logico - Negocio + tec, definição do tipo de relação
Fisico - Implementação

07/06 OK Star

Modelo relacional é bom para operação mais ruim para analises (Problema de concorrência: Operação x Analises)
ETL (D-1, mas varia) do Relacional para DW. ETL do que irei utilizar, não vou pegar todas as tabelas do Relacional, não é replica do Relacional
Kimbal (Star/Snowflake) - Quero responder perguntas do tipo: Como? / Quando? / Quanto? / Porque? / Quem?
Star, tebela fato é associativa, recebe informações de todas as tabelas
Modelo de BI tem que ter dimensão de tempo


18/06 OK Snowflake
Snowflake nas etapas de queries, no frontend, devido a vários joins
Leve dificuldade para o usuário

Star leva dificuldade para o processo de carga, mas facilita para usuário
Por exigir menos joins

Em um modelo ideal, montamos o modelo (Star, Snowflake) e depois fazemos o ETL para pegar esses dados do sistema transacional

Modelagem nos garante performance

21/06 NoSQL


Informação é o dado com contexto

O principal objetivo da modelagem de dados e definir como os dados devem ser representados

## Tipos de Modelos
- Modelo lógico: Junção de aspectos de negocio e tecnologia
- Modelo físico:



# Modelagem de dados estruturados

## Tipos de dados estruturados
![[Tipos de dados estruturados.png]]

Dados mestres são os dados do negocio

Transação é um evento que ocorre em uma data e hora especifica


## Modelos
![[Pasted image 20240530210313.png]]

 Modelo conceitual: Modelo que está mais proximo do negocio, tradução do projeto em uma representação de alto nível

Um conceito ira virar uma entidade

![[Pasted image 20240530210750.png]]
![[Pasted image 20240530211009.png]]
Os dados de professor, sala de aula e aluna se relacionam em determinado tempo e o corre a transição da aula
Dados de transação é o relacionamento de dados mestre

Ver definição de cardinalidade da professora e resumir a aula, minuto 45 mais ou menos
Do lado que sai a cardinalidade, começa com 1, para fazer o fluxo

Na maioria dos casos, o modelo são especificos para um negocio em especifico

![[Pasted image 20240530213616.png]]

O que é atributo pelo minuto 1:20

![[Pasted image 20240530220603.png]]

Entidades + atributos

![[Pasted image 20240530220941.png]]
![[Pasted image 20240530221122.png]]

Componentes do modelo logico no minuto 2:20

Voltar no minuto 2:40

![[Pasted image 20240531070128.png]]

Modelo físico inicia as 3:00



# Aula 07/06
Formas normais:
1. Uma coluna não pode armazenar mais de uma informação
2.
