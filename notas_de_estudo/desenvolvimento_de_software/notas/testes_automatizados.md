Testes automatizados tem como objetivo escalar a aplicação dos testes, com o objetivo, de aumentar a segurança e evitar bugs nos softwares que estamos desenvolvendo


# Objetivos
- [ ] Conhecer os principais testes disponíveis
- [ ] Escrever bons testes


# Materiais utilizados
- [Getting Started With Testing in Python](https://realpython.com/python-testing/#writing-your-first-python-test)
- [Pytest Tutorial – How to Test Python Code](https://www.youtube.com/watch?v=cHYq1MRoyI0&list=PLBg_kS4-INSfIfDlo0KJMh232_WjElTPL)


# Estruturação dos testes
Antes de começar a escrever os testes, devemos ter alguns pontos em mente:
- O que será testado
- Qual tipo de teste iremos utilizar

Com esses pontos em mente, podemos estruturar um teste da seguinte maneira:
- Criar os inputs
- Executar os testes e capturar os outputs
- Comparar os outputs com um resultado esperado, **assertion**

**fixture**, são os dados criados para serem utilizados como input. Uma boa prática, é recusá-los

**Ponto importante**, os testes devem ser reproduzíveis, devemos conseguir reexecutar os testes e possuir os mesmos resultados


# Tipos de teste
Seguindo as boas práticas, devemos separar os testes:

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
        |
        ├── fixtures/
        |   ├── test_basic.json
        |   └── test_complex.json
        |
        ├── __init__.py
        └── test_integration.py
```

## Teste de integração
Teste de múltiplos componentes e a **integração** entre eles. Como muitos componentes são testados, é difícil saber o que causou o problema quando os resultados não saem como o esperado

Pensando nesse problema, em como fazer testes mais isolados, temos **Teste unitário**

## Teste unitário
Teste aplicado em apenas um componente e verifica se ele está tendo o comportamento esperado. Com este tipo de teste, conseguimos rapidamente separar o que está funcionando do que está quebrado

Caso uma parte da aplicação seja muito interconectada ou caso não conseguimos contornar os efeitos colaterais, nesses casos o ideal é utilizar **testes de integração**
