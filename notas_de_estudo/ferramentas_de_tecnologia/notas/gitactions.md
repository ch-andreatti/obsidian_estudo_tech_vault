O Git Actions, também conhecido como GitHub Actions, é uma ferramenta integrada ao GitHub que permite implementar CI/CD em seus repositórios

Cada repositório tem sua própria pipeline de CI/CD, com workflows e jobs contendo várias actions, como preparar ambiente e testar. O Git Actions elimina a necessidade de integrar com serviços externos, oferece 2 mil minutos/mês gratuitos e é fácil de usar

Os principais componentes são: 
- Workflow (automação)
- Actions (tarefas)
- Runner (execução)

# Conceitos técnicos do Git Actions
## Triggers
O trigger é essencial para o funcionamento da pipeline, pois define quando o workflow será executado. A configuração começa com o parâmetro "on", que é obrigatório.

```
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    name: 'Build and Push'
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
```
