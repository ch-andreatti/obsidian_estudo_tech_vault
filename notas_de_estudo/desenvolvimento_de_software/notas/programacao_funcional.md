Esta nota tem como objetivo, apresentar o paradigma de **programação funcional (PF)**:
- Conceitos
- Pontos positivos
- Pontos negativos
- Onde e como utilizar
- Boas praticas
- ...


# Materiais utilizados
- [Functional Programming in Python: When and How to Use It](https://realpython.com/python-functional-programming/#what-is-functional-programming)


# Programação Funcional (FP)
Paradigma de programação, onde organizamos o código por composição de funções e não fazemos a alteração do estado interno. Não fazemos alterações de variáveis, através da composição de funções, retornamos valores novos

Na programação funcional, **funções não têm memória**. Elas recebem tudo que precisam pelos parâmetros e devolvem tudo que produzem pelo retorno

Neste paradigma, o foco é sobre **o que deve ser feito** e não no **como deve ser feito**


# Boas praticas

## Nomenclatura
- Classe: **PascalCase**
- Métodos e Funções:  **snake_case**, quando possível, utilizar verbo como nome
- Atributos de Instância e Variáveis: **snake_case**
- Constantes: **UPPER_SNAKE_CASE**


## Planejamento
Antes de fazer o desenvolvimento, é uma boa pratica fazer o planejamento e o diagrama da solução


# Conceitos

## Função pura
Função que possui as seguintes propriedades:
- Determinismo: Para a mesma entrada, sempre retorna a mesma saída
- Sem efeitos colaterais: Não modifica nada fora do seu próprio escopo. Um efeito colateral é qualquer interação da função com o mundo fora dela

Devemos nos atentar aos parâmetros da função, se estamos passando valores ou referencias para objetos que são mutáveis, como listas por exemplo

Devido o determinismo, podemos ter uma tabela de cache, para armazenar o retorno da função. Assim, evitamos o reprocessamento repetitivo da função, e pegamos o retorno na tabela de cache

Vale ressaltar que podemos utilizar funções impuras na PF, porém, não podemos deixar as impuras vazarem para dentro da lógica. Estratégia:

- Lógica de negócio: Deve ser 100% pura
- Bordas (I/O): Impura por natureza, isolar aqui
- Orquestração: Impura, mas fina e explícita. Conecta funções puras com não puras

Analogia: A cozinha (lógica) é pura. As portas (I/O) são impuras. O que a programação funcional proíbe é misturar os dois sem critério

Exemplo da orquestração de funções puras e não puras
