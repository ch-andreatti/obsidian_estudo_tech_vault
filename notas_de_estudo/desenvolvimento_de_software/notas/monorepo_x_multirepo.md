Esta nota tem como objetivo listar os pontos positivos e negativos das estrategias de **monorepo** e **multirepo**


# Materiais utilizados
- [Monorepo (Por Que Essa Estratégia Funciona em Grandes Empresas?)](https://youtu.be/BbNIuUy_F0w?si=e4JoCGdwAKi25oWP)
- [I used a Monorepo for 12 months - here’s my opinion](https://youtu.be/rcmdyQL2DUM?si=m6-NzbKk1o7-MAjb)


# monorepo

## Vantagens
- Colaboração e discovery
- Reutilização e compartilhamento de código
- Gerenciamento unificado de dependências
- Commits atômicos e alteração em larga escala
- Adoção de padrão de desenvolvimento

## Desvantagens
- Escalabilidade e desempenho
- Gerenciamento de acesso
- Maior chance de efeito colateral


# multirepo

## Vantagens
- Limites melhores definidos
- Deploy independente
- Codebase mais direcionada e fácil de se entender
- Menor risco de efeitos colaterais
- Gestão de acesso

## Desvantagens
- Dificuldade em fazer alteração atômica em múltiplos repositórios
- Compartilho de código, padrões e configurações mais difíceis
- Código duplicado
