# Sistema de origem
Precisamos entender o sistema que iremos utilizar para fazer a coleta dos dados, pois é de lá que os dados serão gerados e serão a nossa fonte. Se os dados vierem comprometidos da fonte e não aplicarmos as devidas correções, todo o nosso trabalho e as futuras etapas serão comprometidas.

Existem vários pontos importantes sobre os sistemas de origem. Para simplificar, a lista abaixo foi retirada do livro **Fundamentals of Data Engineering** e reúne os principais pontos que precisamos entender sobre esses sistemas:
- Entender o que é o sistema de origem (banco de dados, API, aplicação, dispositivo IoT, ...)
- Geração e armazenamento dos dados
	- Entender se os dados são persistidos por um longo período ou excluídos depois de um certo período de tempo
	- Entender se iremos precisar fazer atualizações retroativas ou se os dados são "fechados" depois de um certo período
- Frequência e velocidade dos dados
	- Eventos por segundo
	- Gigabytes por hora
	- Entender as limitações que existem para fazermos o consumo dos dados
	- Entender o atraso dos dados (D-1, M-1, ...)
- Variedade dos dados
	- Formato
	- Possibilidade de data drift
- Nível de consistência dos dados
	- Entender a frequência de atualização do schema (Colunas e tipos)
	- Existência de dados duplicados, nulos, ...
- Taxa de disponibilidade do sistema

**Ponto importante:** O maior desafio será em relação ao schema, ele irá mudar ao longo do tempo e arquitetura deve estar preparada para lidar com essas mudanças.
