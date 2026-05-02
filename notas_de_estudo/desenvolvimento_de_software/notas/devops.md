# Cultura

O DevOps surgiu em 2008 com o objetivo principal de unir as equipes de desenvolvimento e operações, promovendo descentralização de conhecimento e feedback contínuo

Os principais pontos da cultura DevOps são:
- Integração
- Testabilidade
- Descentralização
- Automatização

A cultura DevOps permite experimentar novas tecnologias e melhorar processos

O cargo responsável pela implementação técnica é o Engenheiro de Confiabilidade de Sites (SRE), atua em tarefas como na automatização e agilidade
## CALMS

É uma ferramenta para medir e acompanhar a implementação da cultura DevOps em uma organização
### Culture

Através de um diagnóstico cultural, o CALMS avalia se a empresa já está no modo DevOps ou se ainda está distante. O diagnóstico considera aspectos como a resolução de problemas, a busca por culpados versus a resolução em equipe, e a existência de um processo de pós-mortem para aprender com os erros, ...

O objetivo é identificar falhas nos processos e implementar medidas para mitigá-las, evitando que se repitam no futuro
### Automation

Tudo que pode ser automatizado, deve ser automatizado

O foco está em dois aspectos: entrega contínua e GitOps/IAC

A entrega contínua deve ser automatizada para reduzir o tempo gasto em tarefas repetitivas e garantir agilidade na implantação

Já o GitOps/IAC visa estabelecer uma única fonte de verdade para recursos em nuvem, centralizando o gerenciamento no Git e garantindo maior segurança, economia e manutenibilidade
### Lean

Ser enxuto, visar entregas rápidas e iteráveis, focando em alto valor e baixa complexidade

Através do desenvolvimento de MVPs e features em fatias, o LEAN incentiva uma cultura experimental, permitindo errar rápido e corrigir rápido. É importante observar a aplicação para diagnosticar erros rapidamente e evitar estresse na equipe e nos clientes

O foco deve estar sempre no que traz valor, permitindo avanços mais rápidos e melhores feedbacks
### Measurement

A mensuração envolve métricas negociais e técnicas para avaliar o desempenho da aplicação e tomar decisões informadas, como por exemplo, entender quantas pessoas estão acessando a nova página no meu site e entender o impacto dessa nova feature

O objetivo é descobrir erros antes que impactem os usuários, promover a melhoria contínua e a entrega contínua
### Share

Compartilhamento de conhecimento, visando descentralizar o conhecimento e evitar a Síndrome da Pessoa-Herói

Através da documentação, calls, processos de incidente e mensuração, o conhecimento é disseminado na equipe, liberando tempo para novos aprendizados e impulsionando a produtividade

# Container

Trabalhando com containers, a boa pratica em relação ao uso das tags é utilizar o hash do commit (7 primeiros dígitos)

Quando vamos enviar o container para um registry, é uma boa pratica enviar um container com a tag latest e outro com a tag associado ao último commit

# Infraestrutura como código

Possui os benefícios de:
- Facilitar a manutenção de toda a infraestrutura
- Traz visibilidade do que está sendo utilizado
- Automatiza todo o fluxo de criação, edição e remoção de um recurso

Algumas tecnologias para implementação de IaC são:
- [[terraform]]
- [[terragrunt]]

# CI/CD
## CI

A Continuous Integration (CI) tem o objetivo de mesclar regularmente o código no repositório central, garantindo que pequenas entregas sejam integradas à branch principal

A prática da CI envolve:
- Instalar dependências
- Construir a aplicação 
- Executar testes para validar a integração do código

A CI facilita feedbacks técnicos rápidos (erros são identificados logo no inicio) e contribui para a entrega contínua, sendo um pilar da cultura DevOps
## CD

A Continuous Delivery (CD) tem como objetivo colocar o software em produção ou homologação de forma contínua, após ter sido integrado e testado. É importante ter ambientes de teste e produção semelhantes

A CD é uma extensão da Integração Contínua (CI) e envolve automatizar o processo de implantação do software, permitindo também estratégias como implantação gradual 

A CD é essencial para:
- Automatizar o fluxo de entrega de valor ao cliente
- Facilitar o rollback em caso de problemas
## Geral

Algumas tecnologias para implementação de CI/CD são:
- [[gitactions]]






