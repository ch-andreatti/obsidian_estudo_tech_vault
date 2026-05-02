Esta nota tem o objetivo de ser o hub para os tópicos de **Engenharia de Dados**


# Materiais utilizados
- Fundamentals of Data Engineering: Plan and Build Robust Data Systems
- Data Engineering Design Patterns: Recipes for Solving the Most Common Data Engineering Problems


# Definição
Engenharia de Dados possui o objetivo de desenvolver, implementar e dar manutenção para sistemas e processos que coletam dados e os transformam em informações consistente e de alta qualidade. Essas informações geradas serão a base para cases futuros, como análise de dados e ML. Em resumo Engenharia de Dados é um HUB entre data producers e data consumers

## Fundamentos x Novas tecnologias
Uma sugestão do livro **Fundamentals of Data Engineering** sobre o que focar estudar nos estudos, é focar nos fundamentos e entender o que não vai mudar, frameworks morrem mas fundamentos não

Focar principalmente nos conceitos e conhecer por alto nível as ferramentas que irão ser utilizadas para aplicar esses conceitos, e ir aprofundando nas ferramentas conforme a necessidade. É importante saber também para onde a área de Engenharia de Dados está caminhando

Não devemos deixar o hype guiar os nossos estudos, a adoção de novas tecnologias também não pode seguir o hype, toda adoção de tecnologia deve ser uma decisão estratégica

## Responsabilidades
Para ser um bom Engenheiro de Dados, não devemos apenas dominar a parte técnica, devemos olhar as nossas responsabilidades através de uma visão técnica e do negócio

### Responsabilidades com o negócio
Abaixo está uma lista de responsabilidades retiradas do livro **Fundamentals of Data Engineering**:
- Aprender a se comunicar com pessoas não técnicas
- Entender a cultura da empresa e também Agile, DevOps, DataOps
- Controlar custos
- Nunca parar de aprender


# Ciclo da Engenharia de Dados
![[ciclo_da_engenharia_de_dados.png]]
Um fator determinante para as atividades que um Engenheiro de Dados irá atuar é a maturidade em relação aos dados da empresa, pois em cada nível de maturidade, atividades diferentes serão requiridas. Os níveis de maturidade são:
1. Iniciando com dados: Desenvolver uma boa base para os dados e educar os colaboradores sobre a importância e boas práticas de como utilizarem os dados 
2. Escalando com dados: Desenvolver arquitetura de dados escalável e preparar a empresa para se tornar data driven
3. Liderando com dados: Continuar evolução do passo anterior e aplicar novos conceitos, como DataOps, desenvolver ferramenta de dados, ...

**Observação:** Ver descrição completa de cada nível no livro **Fundamentals of Data Engineering**

Como mostrados acima, o ciclo possui as seguintes etapas:
- [[geracao_de_dados]]
- [[armazenamento_de_dados]]
- [[ingestao_de_dados]]
- Transformação
- Servir os dados
- Undercurrents
	- Segurança
	- Gestão de dados
		- [[qualidade_de_dados]]
	- DataOps
	- [[arquitetura_de_dados]]
	- Orquestração
	- [[desenvolvimento_de_software]]

Cada etapa tem uma papel crucial no ciclo, cada etapa terá sua página para detalhar melhor seu papel e objetivos
