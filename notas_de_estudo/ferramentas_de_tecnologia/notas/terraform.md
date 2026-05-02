É uma ferramenta destinada para aplicar IaC (Infraestrture as code)
# Conceitos
## Estado

O estado é responsável por gravar todas as alterações feitas durante a execução do comando apply, caso o comando plan seja executado ou o apply mas acontece algum erro, o estado não será alterado

Ele mapeia os recursos da infraestrutura e permite o Terraform entender o que foi modificado, criado, editado ou deletado

É recomendado gerenciar o estado remotamente, por exemplo, utilizando o S3 da AWS

O tfsatete.backup será uma versão anterior do tfstate
## Workspace

O workspace no Terraform é o espaço de trabalho onde o gerenciamento de estado (tfstate) fica associado

Pode-se ter vários workspaces, cada um com seu próprio estado (local, staging, produção), onde cada workspace possui os mesmos recursos

Ao trabalhar com o mesmo recurso em diferentes workspaces, é necessário mutabilizar o nome do recurso para evitar conflitos
# Organização dos scripts
## providers
Arquivo responsável por especificar a cloud ou clouds que irão ser utilizadas
## main

### módulos
Será responsável por definir a criação do recurso

Módulos são uma maneira de organizar e reutilizar configurações de forma eficiente, são 
parecidos com funções, recebem variáveis e resolvem um problema em especifico
### main
Será responsável por organizar a criação dos recursos, script onde os módulos são chamados

Utilizamos o comando `module` para chamar os módulos
## datasources
Arquivo responsável por expor dados que serão utilizados para criação/configuração de outros recursos

Utilizamos o comando `data` para expor os dados

Para utilizar datasources o recurso já deve estar criado, pois o datasource irá coletar os dados no cloud provider
## outputs
Arquivo responsável por definir as variáveis de saída pós criação de um recurso

Utilizamos o comando `output` para coletar os dados

Também podemos pegar os dados direto dos recursos, passando o resource e o alias
## variables
Arquivo responsável por definir as varáveis que serão utilizadas

Utilizamos o comando `variable` para definir as varáveis
## terraform.tfvars
Arquivo responsável por definir os valores das varáveis, similar com arquivo `.env`

Podemos definir valores que não devem ficar expostos no repositório do projeto
## .gitignore

Os seguintes arquivos devem ser ignorados:
```
*.tfstate
*.tfstate.*
**/.terraform/*
*.tfvars
```

Existem templates para o .gitignore do Terraform

# Pontos para estudar
- Como organizar ambiente em dev / staging / prod utilizando sistema de pastas, pesquisar por melhores praticas adotadas pelo mercado.
- Como adaptar o Terraform para lidar com recursos já criados pelo console, como fazer o Terraform ficar atualizado sem comprometer a infra já criada.