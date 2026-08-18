Tem o objetivo de garantir que a solução funcione, como o esperado. Para atingir esse objetivo , utilizamos testes automatizados que irão garantir a qualidade da solução. Testes automatizados proporcionam os seguintes benefícios:
- Verificar se a solução está se comportando como esperado
- Pegar bugs antes de irem para produção
- Refatoração simplificada
- Encoraja organizar o código e utilizar abstrações
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

Com os pontos a cima definidos, implementamos o teste da seguinte maneira:
- Definir os inputs
- Desenvolvimento e execução do teste 
- Coleta do output
- Comparação do output com o resultado esperado (**assertion**)
- Reinicialização do ambiente, para não ocorrer interferência entre os testes

Complementando, os testes possuem a seguinte anatomia: [Anatomy of a test](https://docs.pytest.org/en/stable/explanation/anatomy.html#test-anatomy)

Uma boa maneira para começar a testar um componente é pelo teste vanila (caminho feliz), pela situação onde é esperado que o componente irá funcionar. Com o primeiro teste desenvolvido, podemos evoluir para os demais casos

**Ponto importante**: Os testes devem ser reproduzíveis, ou seja, devemos obter o mesmo resultado de um teste quando utilizados os mesmos inputs

## Given When Then
Maneira de estruturar os testes:
- Given: Definir os inputs
- When: Executar a ação que está sendo validada
- Then: Validar se o comportamento foi o esperado

```Python
def test_initial_editor_is_empty():
	# GIVEN: Documento incial após inicialização
	editor = DocumentEditor()
	
	# WHEN: Verificação se editor está vazio
	is_editor_empty = editor.is_empty()
	
	# THEN: editor deve estar vazio
	assert is_editor_empty
```


## Mocking
Em alguns testes, podemos lidar  com alguns recursos que não é interessante a interação, como por exemplo a conexão com um banco de dados ou uma API. Nesses casos, podemos fazer o mocking, para simular a interação com esses recursos

A seguinte seção, mostra como fazer o mocking utilizando o **pytest**: [[#Mocking Pytest]]

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


# pytest


# Geral
- Podemos definir o diretório que o `pytest` será executado com o comando `export PYTHONPATH=.`

## Invocação dos testes
Para a identificação e execução de um teste, o nome é importante. O `Pytest` invoca os testes que seguem os padrões abaixo:
- Módulos: `test_*.py` ou `*_test.py`
- Função e métodos: `test_...`
- Classe: `Test...`


## Modelos de execução do pytest
- `python -m pytest` ou `pytest`
- `pytest -v`: Variação para mostrar mais informações de log
- `pytest --collect-only`: Coletar os testes que serão executados, testes não serão executados
- `pytest --setup-show`:  Mostrar o setup que será executado nos testes
- `pytest --fixtures`: Listar as fixtures definidas
- `pytest --fixtures-per-test`: Listar as fixtures definidas por teste


## Exemplo de teste unitário
**src/math.py**

```Python
def sum_from_list(numbers: list) -> int:
    total = 0
    for n in numbers:
        total += n
    return total

def diff_two_numbers(a: int, b: int) -> int:
    return a - b

def divide_two_numbers(a: int, b: int) -> float:
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

**tests/test_math.py**

```Python
from src.math import sum_from_list, diff_two_numbers, divide_two_numbers
import pytest

def test_sum_from_list():
    assert sum_from_list([1, 2, 3]) == 6
    assert sum_from_list([-1, 1]) == 0

def test_diff_two_numbers():
    assert diff_two_numbers(5, 3) == 2, "Expected 2 when subtracting 3 from 5"
    assert diff_two_numbers(3, 5) == -2, "Expected -2 when subtracting 5 from 3"

def test_divide_two_numbers():
    
    assert divide_two_numbers(6, 3) == 2.0, "Expected 2.0 when dividing 6 by 3"
    assert divide_two_numbers(5, 2) == 2.5, "Expected 2.5 when dividing 5 by 2"

    with pytest.raises(ValueError) as e:
        divide_two_numbers(5, 0)
    assert str(e.value) == "Cannot divide by zero"
```

A fim de deixar os nossos testes melhores organizados, podemos agrupar os testes do mesmo assunto em uma classe:

```Python
class TestClass:
    def test_one(self):
        x = "this"
        assert "h" in x
        
    def test_two(self):
        x = "hello"
        assert hasattr(x, "check")
```


# Fixtures
Responsável pela etapa de preparação, onde é configurado o ambiente necessário para determinado teste, antes da execução. Algumas etapas são:
- Definição dos inputs
- Exclusão de estados anteriores, para não haver interferência entre os testes
- ...

Em resumo, fixtures são os inputs que serão utilizados em múltiplos testes

Utilizando **pytest**, podemos utilizar fixtures da seguinte maneira: 
- [About fixtures](https://docs.pytest.org/en/stable/explanation/fixtures.html)
- [Fixtures reference](https://docs.pytest.org/en/stable/reference/fixtures.html#fixture-availability)
- [How to use fixtures](https://docs.pytest.org/en/stable/how-to/fixtures.html#fixture-scopes)

Um ponto importante é definir corretamente o **scope** da fixture, que será responsável pelo seu ciclo de vida

## Setup e Teardown
Estrategia para garantir que um teste não interfira no outro:
- Setup: Etapa onde os inputs são definidos, esta etapa é executada antes dos testes
- Teardown: Etapa onde o ambiente é reinicializado (Exclusão de arquivo, finalização da conexão com banco, ...), para o próximo teste não ser afetado com a execução do anterior. Esta etapa é executada após finalização do teste

```Python
import os
import pytest

@pytest.fixture
def temp_file():
	
    # Setup: Create a temporary file
    file_path = "temp_file.txt"
    if not os.path.exists(file_path):
        with open(file_path, "w") as f:
            f.write("Hello, world!")
    
    yield file_path
    
    # Teardown: Remove the temporary file
    os.remove(file_path)

def test_temp_file_exists(temp_file):
    assert os.path.exists(temp_file), "Temporary file should exist"
```

No pytest, utilizamos **yield** para habilitar a logica do **Teardown**


## conftest
Recurso onde podemos centralizar as fixtures em um arquivo `conftest.py` e reaproveitar elas em diversos testes. Não é necessário fazer o import das fixtures, o pytest consegue descobrir elas automaticamente

Vale ressaltar que o arquivo `conftest.py` deve estar no mesmo nível ou a cima, dos arquivos de teste

# Mocking Pytest
Utilizando **pytest**, podemos fazer mocking da seguinte maneira: 
- [How to monkeypatch/mock modules and environments](https://docs.pytest.org/en/stable/how-to/monkeypatch.html#how-to-monkeypatch-mock-modules-and-environments)
- [monkeypatch](https://docs.pytest.org/en/stable/reference/reference.html#pytest.MonkeyPatch)

## Links úteis
- [How to invoke pytest](https://docs.pytest.org/en/stable/how-to/usage.html)
- [How to write and report assertions in tests](https://docs.pytest.org/en/stable/how-to/assert.html#assertraises)

TIRAR ESSA SEÇÃO
