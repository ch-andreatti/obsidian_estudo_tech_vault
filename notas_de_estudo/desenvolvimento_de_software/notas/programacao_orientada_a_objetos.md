Esta nota tem como objetivo, apresentar o paradigma de **orientação a objetos (POO)**:
- Conceitos
- Pontos positivos
- Pontos negativos
- Onde e como utilizar
- Boas praticas
- ...


# Materiais utilizados
- [Python Object Oriented Programming (OOP) - Full Course for Beginners](https://youtu.be/iLRZi0Gu8Go?si=pu-3YwssaTejhOjG)
- [Python OOP: Object Oriented Programming From Beginner to Pro](https://www.udemy.com/share/10cqlZ3@C7BByR2uBn9ls00ZKUUQXxklk0OpfytGrxn3CO-26Tu4mQ256DajhvTzkFMzv-ux/)
	- [GitHub](https://github.com/DoableDanny/oop-in-python-course/tree/main)


# Programação Orienta a Objetos (POO)
É um paradigma de programação com o objetivo de organizar um software de forma mais clara, modular e fácil de manter.

Pense na POO como **montar um sistema usando peças de LEGO**. Cada peça (classe/objeto) tem uma função específica e pode ser combinada com outras para criar algo maior. Utilizando POO, temos o objetivo de:
- **Organizar o código em estruturas que representem o mundo real**
	- Você modela coisas como: Usuário, Pedido, ContaBancária, Cliente, ...
	- Analogia: Como um engenheiro cria plantas com “salas”, “portas”, “janelas”, você cria classes que representam entidades
- **Reutilizar código**
	- Uma classe criada uma vez pode ser usada em várias partes do sistema (instâncias)
	- Analogia: Criar um molde de bolo. Depois você faz quantos bolos quiser
- **Facilitar manutenção e evolução do código**
	- Cada classe tem uma responsabilidade clara. Se algo quebrar, você sabe onde mexer
	- Analogia: Como revisar uma máquina industrial com peças modulares, não precisa desmontar tudo
- **Reduzir complexidade**
	- Divide um problema grande em vários problemas pequenos e controláveis
	- Analogia: Quebrar um projeto de engenharia em módulos: fundação, estrutura, elétrica, hidráulica…


# Boas praticas

## Nomenclatura
- Classe: **PascalCase**
- Métodos e Funções:  **snake_case**, quando possível, utilizar verbo como nome
- Atributos de Instância e Variáveis: **snake_case**
- Constantes: **UPPER_SNAKE_CASE**

## Planejamento
Antes de fazer o desenvolvimento, é uma boa pratica fazer o planejamento e o diagrama da solução

Uma ferramenta que pode nos ajudar nesta etapa, é a [[uml]]


# Conceitos

## Encapsulamento
Esconder detalhes de implementação e controlar acesso, com foco na proteção de dados e estado interno

Fazemos a definição se os atributos e métodos serão públicos, protegidos ou privados e também como os mesmos devem ser acessados e alterados 

Analogia: Medicamento dentro da cápsula (Protegido), só pode ser aberto de forma específica (Acesso controlado)

```python
class BankAccount:
    def __init__(self):
        self._balance = 0.0
        
    @property
    def balance(self):
        return self._balance
        
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        self._balance += amount
        
    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdraw amount must be positive")
        if amount >= self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount
```

## Abstração
Esconde implementações internas, expondo apenas o que é necessário. A classe decide o que é exposto ou escondido, isso evita erros e mantém consistência. Foco é na interface e funcionalidade

Não fazemos a exposição de todos os métodos, expomos apenas os métodos que possuem funcionalidades reais, métodos intermediários deixamos como protegidos ou privados

Analogia: Você usa um micro-ondas sem precisar saber como os circuitos internos funcionam

```python
# ❌ Errado

class BadEmailService:
    
    def connect(self):
        print("Connecting to email server...")
        
    def authenticate(self):
        print("Authenticating...")
        
    def send_email(self):
        print("Sending email...")
        
    def disconnect(self):
        print("Disconnecting from email server...")

email = BadEmailService()

email.connect()
email.authenticate()
email.send_email()
email.disconnect()
```

```python
# ✅ Correto

class EmailService:
    def send_email(self):
        self._connect()
        self._authenticate()
        print("Sending email...")
        self._disconnect()
        
    def _connect(self):
        print("Connecting to email server...")
        
    def _authenticate(self):
        print("Authenticating...")
        
    def _disconnect(self):
        print("Disconnecting from email server...")


email = EmailService()
email.send_email()
```

## Herança
Criação de classe a partir de outra já existente, possibilitando:
- Reutilização dos atributos e métodos da classe base, reutilização de código
- Desenvolvimento apenas dos atributos e métodos específicos da classe

Utilizando **Herança**, conseguimos desenvolver classes especificas sem gerar código duplicado

Um indicativo de que vamos precisar de herança, é quando as classes possuem a relação de **é um**. Exemplo: cachorro **é um** animal

Mas devemos nos atentar que quando usamos **herança**, estamos aumentando o **acoplamento** das classes

## Polimorfismo
Recurso que um objeto pode possuir diversas formas, através da sobrescrita dos métodos da classe base (**Herança**)

É a capacidade de um objeto assumir **muitas formas** e se comportar de maneiras diferentes dependendo do contexto

```python
# Parent class ("Superclass")
class Vehicle:
    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year
        
    def start(self):
        print("Vehicle is starting.")
        
    def stop(self):
        print("Vehicle is stopping.")

# Child class ("Subclass") of Vehicle superclass
class Car(Vehicle):
    def __init__(self, brand, model, year, number_of_doors):
        super().__init__(brand, model, year)
        self.number_of_doors = number_of_doors
        
    def start(self):
        print("Car is starting.")
        
    def stop(self):
        print("Car is stopping.")

# Child class ("Subclass") of Vehicle superclass
class Motorcycle(Vehicle):
    def __init__(self, brand, model, year):
        super().__init__(brand, model, year)
        
    def start(self):
        print("Motorcycle is starting.")
        
    def stop(self):
        print("Motorcycle is stopping.")
```

## Acoplamento
Verifica a dependência que às classes possuem entre si. Em classes altamente acopladas, a alteração em um classe pode quebrar outra

Idealmente, as classes devem poder receber alterações de maneira isolada, ou seja, a alteração de uma classe não deve gerar o retrabalho de alterar outra que a use

## Composição
Criação de um objeto complexo, a partir de múltiplos objetos. Como por exemplo, montar um objeto carro, onde esse objeto será constituído pelos objetos:
- Motor
- Roda
- Banco
- ...

```python
class Engine:
	def start(self):
		print("Starting engine")

class Wheels:
	def rotate(self):
		print("Rotate")

class Chassis:
	def support(self):
	    print("Chassis supporting the car")

class Seats:
	def sit(self):
	    print("Sitting on seats")

# Car is composed (made up from) of the above components
class Car:
    def __init__(self):  
        self._engine = Engine()
        self._wheels = Wheels()
        self._chassis = Chassis()
        self._seats = Seats()
        
    def start(self):
        self._engine.start()
        self._wheels.rotate()
        self._chassis.support()
        self._seats.sit()
        print("Car started.")
```

## Fragile base class
Problema onde alterações na classe base, quebra as funcionalidades das subclasses. Esse problema é causado devido ao alto **acoplamento** entre as classes


# SOLID
É um acrônimo de **cinco princípios de design orientado a objetos**, formulados por Robert C. Martin (Uncle Bob), que visam tornar o software mais legível, escalável e facilitar a manutenção.

Podemos destacar os seguintes pontos positivos e negativos:
- Positivos
	- Manutenção: Código mais fácil de corrigir e evoluir
	- Testabilidade: Classes isoladas são mais fáceis de testar unitariamente
	- Reusabilidade: Componentes coesos podem ser reaproveitados
	- Legibilidade: Código mais claro e com intenção bem definida
	- Extensibilidade: Novos comportamentos sem risco de quebrar o existente
	- Baixo acoplamento: Mudanças em uma parte impactam menos o restante
- Negativos
	- Overengineering: Em projetos pequenos, pode gerar complexidade desnecessária
	- Curva de aprendizado: Exige maturidade para aplicar corretamente sem exagerar
	- Abstração excessiva: Camadas demais podem dificultar o rastreamento do fluxo
	- Tempo inicial maior: Planejar bem a arquitetura demanda mais esforço no início

É indicado aplicar para:
- Projetos **médios e grandes** com múltiplos desenvolvedores
- Sistemas que precisam **escalar e evoluir** ao longo do tempo
- Quando há **testes automatizados** no projeto

Vale lembrar que o **SOLID** não é uma lei absoluta, é um **guia**. O objetivo final é sempre um código que seja fácil de entender, mudar e testar

## Princípios

### Single Responsibility Principle
Princípio da **responsabilidade única**, uma classe deve ter apenas um motivo para mudar. Cada classe deve fazer **uma única coisa** bem feita

```python
# ❌ Errado: classe com múltiplas responsabilidades
class Relatorio:
    def gerar_dados(self): ...
    def formatar_html(self): ...
    def enviar_email(self): ...

# ✅ Correto: responsabilidades separadas
class GeradorDeDados: ...
class FormataRelatorio: ...
class EnviadorDeEmail: ...
```

### Open/Closed Principle
Devemos conseguir **adicionar comportamentos sem alterar o código existente**. O código deve estar aberto para extensão e fechado para modificação

```python
# ✅ Extensível via herança/polimorfismo
class Desconto:
    def calcular(self, valor): ...

class DescontoVIP(Desconto):
    def calcular(self, valor): return valor * 0.8

class DescontoEstudante(Desconto):
    def calcular(self, valor): return valor * 0.9
```

### Liskov Substitution Principle (LSP)
Qualquer subclasse deve poder substituir sua classe base, sem quebrar o sistema. Todos os recursos da classe base, devem ser funcionais e fazerem sentido nas subclasses

"Se `S` é um subtipo de `T`, então objetos do tipo `T` podem ser substituídos por objetos do tipo `S` sem alterar as propriedades do programa."

Se você precisa sobrescrever um método para lançar exceção ou não fazer nada, é um sinal claro de que a herança está errada e o LSP está sendo violado. A solução quase sempre é repensar a hierarquia usando abstrações mais adequadas

```python
# ❌ Errado: Pato de borracha não voa, mas herda voar()
class Pato:
    def voar(self): ...

class PatoDeBorracha(Pato):
    def voar(self): 
	    raise Exception("Não posso voar!")

# ✅ Correto: separar a capacidade de voar
class Ave: ...

class AveQueVoa(Ave):
    def voar(self): ...

class PatoDeBorracha(Ave): ...  # não herda voar()
```

### Interface Segregation Principle (ISP)
Classes não devem ser forçadas a dependerem de interfaces que elas não usam ou que não fazem sentido

Nenhuma classe deve ser forçada a implementar métodos que não serão utilizados

Prefira várias interfaces pequenas e específicas a uma interface grande e genérica

```python
from abc import ABC, abstractmethod

# ❌ Interface grande e genérica
class Trabalhador(ABC):
    @abstractmethod
    def trabalhar(self): pass
    
    @abstractmethod
    def comer(self): pass
    
    @abstractmethod
    def dormir(self): pass


class Humano(Trabalhador):
    def trabalhar(self): 
	    print("Humano trabalhando...")
    
    def comer(self):    
	    print("Humano comendo...")
    
    def dormir(self):   
	    print("Humano dormindo...")


class Robo(Trabalhador):
    def trabalhar(self): 
	    print("Robô trabalhando...")
    
    # ❌ violação
    def comer(self):
	    raise NotImplementedError("Robô não come!") 
    
    # ❌ violação
    def dormir(self):
	    raise NotImplementedError("Robô não dorme!")
```

### Dependency Inversion Principle (DIP)
Dependa de abstrações, não de implementações concretas

```python
# ❌ Acoplamento rígido

class MySQL:
    def salvar(self, dado): 
	    print(f"Salvando '{dado}' no MySQL")


class PostgreSQL:
    def salvar(self, dado): 
	    print(f"Salvando '{dado}' no PostgreSQL")

class Pedido:
    def __init__(self):
        self.db = MySQL()  # E se quiser trocar para PostgreSQL? 💥
        
    def realizar(self, dado):
        self.db.salvar(dado)
```

```python
# ✅ Dependencia de abstrações

from abc import ABC, abstractmethod

class BancoDeDados(ABC):
    @abstractmethod
    def salvar(self, dado: str): pass


class MySQL(BancoDeDados):
    def salvar(self, dado: str):
        print(f"Salvando '{dado}' no MySQL")


class PostgreSQL(BancoDeDados):
    def salvar(self, dado: str):
        print(f"Salvando '{dado}' no PostgreSQL")


class BancoEmMemoria(BancoDeDados):  # Ótimo para testes!
    def salvar(self, dado: str):
        print(f"Salvando '{dado}' em memória")


class Pedido:
    def __init__(self, db: BancoDeDados):  # Injeção via construtor
        self.db = db
        
    def realizar(self, dado: str):
        self.db.salvar(dado)


# Quem chama decide qual implementação usar
pedido1 = Pedido(MySQL())
pedido1.realizar("Pedido #1")

pedido2 = Pedido(PostgreSQL())
pedido2.realizar("Pedido #2")
```

#### Injeção de dependência
Em vez de uma classe criar suas próprias dependências, elas são fornecidas externamente

```python
# As 3 formas de injetar dependência

# ✅ 1. Via construtor (mais comum e recomendada)
class Servico:
    def __init__(self, repositorio: BancoDeDados):
        self.repositorio = repositorio


# ✅ 2. Via método setter
class Servico:
    def set_repositorio(self, repositorio: BancoDeDados):
        self.repositorio = repositorio


# ✅ 3. Via parâmetro do método (mais pontual)
class Servico:
    def executar(self, repositorio: BancoDeDados, dado: str):
        repositorio.salvar(dado)
```


# Implementação

## Atributos
São as variáveis da classe. Podemos proteger atributos de duas maneiras:
- Atributos protegidos: Utilizando `_`, exemplo: `self._atributo`. Ainda conseguimos acessar o atributo, mas é uma boa pratica não acessar diretamente atributos protegidos
- Atributos privados: Utilizando `__`, exemplo: `self.__atributo`. Devido ao **name mangling**, não conseguimos acessar esses atributos, de maneira convencional

### Atributos de instância
São acessados via `self`, ou seja, uma **referência à instância** do objeto. Utilizados para controlar o estado da instância

```python
class ContaBancaria:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo
```

Esses atributos ficão a nível de instância

### Atributos de classe
São acessados utilizando a classe. Esses tipos de atributos que ficam armazenados em todas as instâncias da classe

```python
class ContaBancaria:
    taxa_juros = 0.02

    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo
```

## Métodos
São as funções dentro das classes, abaixo estão descritos os principais tipos de métodos

Podemos proteger os **métodos**, da mesma forma que protegemos os **atributos**

### Métodos de instância
São as funções que recebem o parâmetro `self`, ou seja, uma **referência à instância** do objeto 

Tem o objetivo de acessar e modificar atributos da instância, usados quando o comportamento depende do estado específico do objeto.

```python
class ContaBancaria:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo

    # Método de instância
    def depositar(self, valor):
        self.saldo += valor

c = ContaBancaria("Caio")
c.depositar(100)
```

#### getters e setters (`@property`)
**Métodos de instância** que controlam o acesso e a modificação dos atributos

Usados quando você quer **encapsular lógica** de acessar ou modificar atributos

```python
class Conta:
    def __init__(self, saldo):
        self._saldo = saldo

    @property # getter
    def saldo(self):
        return self._saldo

    @saldo.setter # setter
    def saldo(self, valor):
        if valor < 0:
            raise ValueError("Saldo não pode ser negativo")
        self._saldo = valor
        
c = Conta(100)
print(c.saldo)  # sem parênteses, devido o @property/criação do getter 
c.saldo = 200   # chama o setter
print(c.saldo)  # chama o getter
```

No **getter**, permitimos acesso com validação e logs.

No **setter**, permitimos alteração com validação e logs.

Pontos importantes, a ordem **sempre deve ser**:
1. Primeiro definimos o `getter`
2. Depois definimos o `setter`

Isso porque internamente, quando usamos `@property`, o Python cria um objeto especial da classe `property`.  O `@saldo.setter` simplesmente **anexa** a função `saldo(self, valor)` como **setter** desse mesmo objeto

### Métodos de classe (`@classmethod`)
São métodos que recebem `cls` (a **classe**) em vez de `self` (instância).  Podem **criar instâncias** ou **alterar comportamentos da classe como um todo**. Seu uso comum é:
- Criar **métodos fábricas** (formas alternativas de criar objetos)
- Trabalhar com informações que afetam **todas as instâncias**

```python
class ContaBancaria:
    taxa_juros = 0.02

    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo

    @classmethod
    def alterar_taxa(cls, nova_taxa):
        cls.taxa_juros = nova_taxa
        print(f"Nova taxa definida: {cls.taxa_juros}")

ContaBancaria.alterar_taxa(0.03)
```

Todas as instância da classe `ContaBancaria`, seriam alteradas para o novo valor do atributo `taxa_juros`

### Métodos estáticos (`@staticmethod`)
São métodos dentro da classe que não recebem `self` nem `cls`.  Eles **não acessam atributos de instância nem de classe**, só estão dentro da classe por **organização lógica**.

Seu uso comum é em criar **funções utilitárias** relacionadas à classe, mas que **não dependem de estado**.

```python
class ContaBancaria:
    
    @staticmethod
    def validar_valor(valor):
        return valor > 0

print(ContaBancaria.validar_valor(100))  # True
print(ContaBancaria.validar_valor(-50))  # False
```

Usamos `@staticmethod` quando o método **não precisa saber nada** sobre a **classe** ou **instância**.

## Classe abstratas
Classes que servem como modelo, para outras classes. Utilizamos classes abstratas, para garantir que as subclasses vão possuir determinados métodos 

Garantimos essa **interface** em comum, através de métodos abstratos. Onde apenas definimos eles na classe abstrata, e nas classes filhas, esses métodos terão que ser implementados

```python
from abc import ABC, abstractmethod

# Interface for notifications (polymorphism and dependency injection = FLEXIBLE!)
class NotificationService(ABC):
    """Interface-like base class for notification services."""
    
    @abstractmethod
    def send_notification(self, message: str):
        """Abstract method to send a notification."""
        pass
        
class EmailService(NotificationService):
    def send_notification(self, message: str):
        print(f"Sending email: {message}")
        
class MobileService(NotificationService):
    def send_notification(self, message: str):
        print(f"Sending text message: {message}")
        
# In this class, we introduce dependency injection and Python's type hinting
class Order:
    def __init__(self, notification_service: NotificationService):
        self.notification_service = notification_service
        
    def create(self):
        self.notification_service.send_notification(
            "Hi, your order was placed successfully and will be with you within 2-5 working days"
        )
        
order = Order(EmailService())
order.create()

order2 = Order(MobileService())
order2.create()
```
