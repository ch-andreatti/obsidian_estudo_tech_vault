Tem o objetivo de garantir que a solução funcione, como o esperado. Para atingir esse objetivo utilizamos testes automatizados, que irão nos proporcionar as seguintes vantagens:
- Verificar se nosso software está se comportando como esperado
- Verificar se uma alteração irá causar algum efeito colateral no sistema
- ...

No começo ira exigir um pouco de tempo, para configurar o ambiente dos testes. Mas no longo prazo, tende a agilizar as entregas, dar mais segurança para o time em fazer alterações e evitar bugs


# Materiais utilizados
- [Getting Started With Testing in Python](https://realpython.com/python-testing/#writing-your-first-python-test)
- [Pytest Tutorial – How to Test Python Code](https://www.youtube.com/watch?v=cHYq1MRoyI0&list=PLBg_kS4-INSfIfDlo0KJMh232_WjElTPL)
- [Pytest documentation](https://docs.pytest.org/en/stable/index.html)
- [Learn Pytest Framework: Python Automation Testing, Unit Testing, API Testing & Test Automation with GitHub Actions CI/CD](https://www.udemy.com/share/10eIC93@wYEZeO7ekrTRTvlhYld6gITksFYTDCYl1cAVnEWv8QrNci6z1TdgFkjlThDEUxqR/)


# Estruturação dos testes
Antes de escrever um teste, devemos definir os seguintes pontos:
- O que será testado
- Qual tipo de teste será utilizado

Com os pontos a cima definidos, estruturamos o teste da seguinte maneira:
- Criação dos inputs
- Execução do teste e captura dos outputs
- Comparação dos outputs com o resultado esperado via **assertion**

Uma boa maneira para começar a testar um componente é pelo caminho feliz, pelo caminho onde é esperado que o componente irá funcionar. Com o teste vanilla desenvolvido, podemos evoluir para os demais casos

**Ponto importante**: Os testes devem ser reproduzíveis, ou seja, devemos obter o mesmo resultado de um teste quando utilizado os mesmos inputs


# Tipos de testes

## Teste unitário
Teste aplicado em apenas um componente e verifica se ele está tendo o comportamento esperado. Com este tipo de teste, conseguimos rapidamente separar o que está funcionando do que está quebrado

Caso uma parte da aplicação seja muito interconectada ou caso não conseguimos contornar os efeitos colaterais, nesses casos o ideal é utilizar **testes de integração**


## Teste de integração
Teste de múltiplos componentes e a **integração** entre eles. Como muitos componentes são testados, é difícil saber o que causou o problema quando os resultados não saem como o esperado

Pensando nesse problema, em como fazer testes mais isolados, temos **Teste unitário**


# Desenvolvimento dos testes automatizados

## Organização dos testes
```
project/
│
├── my_app/
│   └── __init__.py
│
└── tests/
    |
    └── unit/
    |   ├── __init__.py
    |   └── test_sum.py
    |
    └── integration/
        ├── __init__.py
        └── test_integration.py
```


## Pytest

### Geral
- Podemos definir o diretorio que o `pytest` será executado com o comando `export PYTHONPATH=.`


### Invocação dos testes
Para a identificação e execução de um teste, o nome é importante. O `Pytest` invoca os testes que seguem os padrões abaixo:
- Módulos: `test_*.py` ou `*_test.py`
- Função e métodos: `test_...`
- Classe: `Test...`

Documentação oficial: [How to invoke pytest](https://docs.pytest.org/en/stable/how-to/usage.html)


### Execução do Pytest
- `python -m pytest` ou `pytest`
- `pytest -v`: Variação para mostrar mais informações
- `pytest --collect-only`: Método para coletar os testes que serão executados, testes não serão executados


### Exemplo de teste unitário
**src/math.py**

```Python
def sum_from_list(numbers: list) -> int:
    total = 0
    for n in numbers:
        total += n
    return total

def diff_two_numbers(a: int, b: int) -> int:
    return a - b
```

**tests/test_math.py**

```Python
from src.math import sum_from_list, diff_two_numbers

def test_sum_from_list():
    assert sum_from_list([1, 2, 3]) == 6
    assert sum_from_list([-1, 1]) == 0

def test_diff_two_numbers():
    assert diff_two_numbers(5, 3) == 2
    assert diff_two_numbers(3, 5) == -2
```
