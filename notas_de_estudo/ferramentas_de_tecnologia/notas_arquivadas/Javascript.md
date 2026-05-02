
**Data do arquivamento:** 11/08/2025

# Boas práticas
- O padrão de escrita é o **camelCase**, um estilo de escrita onde as palavras são unidas e a primeira letra de cada palavra, exceto a primeira, é maiúscula. `exemploDeCamelCase`
- Para o código JavaScript que for executado em uma página web, o recomendado é adicioná-lo no final da página. Dessa maneira, se o arquivo for pesado, ele não irá impedir o carregamento dos outros elementos e não irá causar a sensação de lentidão para o usuário
- Utilizar **strict mode**
# Conceitos da linguagem
## Geral
- Template literals: Recurso para concatenar varáveis com strings, exemplo: (acento grave) Modelo de ${template string} (acento grave)
- Comparação: Utilizando `==` ou `!=` apenas o conteúdo será comparado. Utilizando `===` ou `!==` o conteúdo e o tipo serão comparados
## Características da linguagem
- **single threaded:** Executa uma coisa por vez
- **no-blocking:** Não trava o contexto de execução
- **asynchronous:** Por ser **no-blocking**, precisa utilizar um paradigma assíncrono para lidar com a execução das coisas
- **concurrent:** As tarefas que executam assincronamente, concorrem uma com as outras pelo processamento
## Hoisting
Hoisting significa levantar ou içar e está relacionado ao comportamento do JavaScript de mover suas declarações (variáveis e funções) para o topo do escopo, assim, o Hoisting permite que variáveis e funções sejam utilizadas antes de serem declaradas

Vale ressaltar que as variáveis devem ser declaradas antes de serem utilizadas
## strict mode
Ao ser ativado, torna os erros que eram silenciosos em exceções. Ele é ativado com o comando: `"use strict"`, podemos utilizá-lo em escopo global e local
## Event loop
Modelo de concorrência responsável por gerenciar a execução de código assíncrono e eventos em uma única thread, permitindo que o JavaScript seja não bloqueante
# Sintaxe

## Variáveis
Formas de declarar variáveis em ordem de recomendação:
- `const`
- `let`
- `var`

Variáveis declaradas com `var` têm escopo global, enquanto `let` e `const` têm escopo de função e de bloco, proporcionando um controle mais preciso. Recomenda-se o uso de `let` e `const` em vez de `var` devido a desvantagens como falta de escopo de bloco adequado
## Funções
Formas de declarar funções:
- `function`
- `const/let` + `function`: Função anônima, onde uma `function` é atribuída a uma variável `const/let`
- `=>`: Arrow function, uma forma mais enxuta de escrever funções anônimas. Modelo: `variável = (parâmetros) => {definição da função}`
- `constructor`: Função especial que é executada automaticamente ao instanciar uma classe

Funções declaradas podem ser escritas após a sua definição, devido ao `hoisting`

Funções são documentadas seguindo o padrão de comentário `/** */` (JSDoc comment), onde o objetivo, parâmetros e retorno serão descritos

**Callback function** é uma função passada para outra na forma de parâmetro, passamos ela sem os (), porém, para executá-la devemo adicioná-los

Quando estamos trabalhando com **módulos**, devemos utilizar o `export` para indicar quais funções poderão ser importadas, as demais serão consideradas como restritas. O `export` pode ser adicionado nas funções ou na parte final, exportando múltiplas funções

Utilizando o `setTimeout` podemos executar uma função após um intervalo de tempo determinado em milissegundos. O `setTimeout` recebe como parâmetros a função a ser executada e o tempo em milissegundos

Utilizando o `setInterval` podemos executar uma função repetidamente em intervalos definidos. Recebe como parâmetros a função a ser executada e o tempo em milissegundos
## Funções assíncronas
São funções que não retornam o resultado de maneira imediata, elas retornam **promises** ao invés de valores imediatos. Funções assíncronas são úteis quando precisamos lidar com operações que podem demorar, como acessar um banco de dados

Trabalhamos com o retorno das **Promises** através dos seguintes métodos:
- `resolve`: Método que será executado quando a promise tiver sucesso
- `reject`: Método que será executado quando a promise falhar
- `then`: Este método só será executado, se o `resolve` ocorrer, se a promises tiver tido sucesso
- `catch`: Este método só será executado, se o `reject` ocorrer, se a promises tiver falhado
- `finally`: É executado independente se a **promise** deu certo ou errado
O uso correto desses métodos permite controlar o fluxo de execução de operações assíncronas

Outra maneira de trabalhar com o retorno de **Promises** é utilizando `async` e `await`. O `async` é utilizado antes do `function` ou dos `()` da arrow function e o `await` antes da chamada da função assíncrona. Com esses dois parâmetros conseguimos aguardar pela resolução da **Promise** antes de continuar a execução do código
## Condicionais e controles de fluxo
- Operador condicional ternário: If e Else executado na mesma linha seguindo o modelo: `comparação lógica ? resultado se verdadeiro : resultado se falso`
- `if`
- `if else`
- `if else if else`
- `switch case`: Essa estrutura é utilizada para analisar diferentes casos e tomar ações específicas para cada um deles. E importante o uso da palavra-chave `break` para interromper a execução após um caso ser encontrado e também a utilização do bloco `default` para lidar com casos não previstos. A estrutura switch case é comumente utilizada para análise de múltiplos casos em programação
- `try catch finally`
- `break`: Encerra a execução de repetições
- `continue`: Pula uma iteração e vai para a próxima, sem encerrar o loop inteiro como o `break` faz
## Repetições e iterações
- `while`: Executa um bloco de código enquanto uma condição é verdadeira.
- `do while`: Executa um bloco de código enquanto uma condição é verdadeira, verificando a condição no final. A diferença do `do while` é que ele executa antes de verificar a condição
- `for`: Executa um bloco de código até que a condição especificada seja falsa. Possui os parâmetros de variável de controle, condição e incremento/decremento para controlar a condição
- `for in`: Executa iterações a partir de um objeto, percorrendo suas propriedades
- `for of`: Executa iterações sobre os valores de um objeto iterável. Diferente do `for in`, o `for off` itera sobre os valores, não sobre as propriedades
## Operadores
- `??`: Operador de coalescência nula, retorna o operando do lado direito quando o do lado esquerdo é `null` ou `undefined`. Modelo: `variavel ?? "Conteúdo null ou undefined"`
- `...`: O operador de `spread` permite expandir o conteúdo de um objeto iterável, como um array ou uma string, para ser usado em zero ou mais argumentos. Modelo: `...objeto_que_sera_desestruturado`
## Dados complexos

Com desestruturação (destructuring assignment), podemos extrair dados de arrays ou objetos em variáveis distintas
### Array
Array é uma coleção ordenada de valores, onde cada item possui uma posição específica chamada de índice

Os construtores do array são: `Array()` e colchetes (similar com a tupla do python)

Funções podem ser armazenadas dentro de arrays

Alguns métodos:
- `map`: Aplica uma função de callback (Anônima or Arrow function) para executar em cada elemento do array original, criando um novo array com base nos retornos
- `filter`: Cria um novo array com elementos que atendem a uma condição
- `every`: Verifica se todos os elementos de um array atendem a uma condição, retornando um valor booleano
- `some`: Verifica se pelo menos um elemento em um array atende a uma condição definida
- `reduce`: Utilizado para reduzir um array a um único valor, como por exemplo, somar todos os itens de uma lista. Ideal para reduções mais complexas, por ter maior flexibilidade
### Objeto
Possui a estrutura do **json**, composto por chave e valor. Nele conseguimos definir propriedades e funções (Função anônima ou Arrow function)
```
conts user = {
	email: "user@email.com",
	age: 18,
	message: function (){
		console.log("Hello user")
	}
}
```

Em objetos, o padrão de escrita das chaves é o **snake_case**

Utilizamos o `this` para acessar propriedades e métodos do objeto

Podemos utilizar o **Optional Chaining** para acessar propriedades e métodos do objeto de maneira segura, evitando erros caso eles não existam. Caso uma propriedade ou método não exista, será retornado o valor `undefined`
```
obj?.property
obj.function?.()
```

Podemos definir **funções construtoras**, que são funções (function) que criam um objeto e o retornam

Para instanciar novos objetos com a função construtora, utilizamos `new` antes da função
### Classes
Classes são uma forma de criar objetos e definir comportamentos usando construtores e métodos. Elas oferecem uma sintaxe mais amigável para criar objetos e herdar protótipos, conhecida como "syntax sugar". As classes têm construtores e métodos, permitindo a criação de modelos para objetos. Além disso, as classes possibilitam a herança de propriedades e métodos de outras classes, promovendo a reutilização de código.

O padrão de escrita é o **PascalCase**

Utilizamos o `this` para acessar propriedades e métodos da classe

Não precisamos da palavra `function` para criar métodos

Com métodos estáticos, podemos utilizamos sem realizar a instancia da classe. Definimos um métodos com estático adicionando a palavra `static` antes de sua criação
## Imutabilidade
Quando algo imutável é criado, ele não pode ser alterado (valores ou propriedades). Em vez disso, é gerado uma cópia com alterações e o original permanecesse inalterado

Oferece benefícios de performance, pela facilidade de identificar alterações

Tipos de copia:
- **shallow copy**: Não copia itens aninhados, para os itens aninhados será criado uma referencia, o que irá alterar o objeto que foi copiado, ideal para objetos simples
- **deep copy**: Copia tudo, ideal para objetos complexos

Podemos copiar das seguintes maneiras:
- spread operator: Um objeto ou array é desestruturados dentro de um novo, exemplo: `{ ...objeto_que_sera_copiado }` e `[ ...array_que_sera_copiado ]`. Objetos/arrays aninhados devem seguir o mesmo processo

Para evitar que um objeto seja modificado de maneira indesejada, podemos congelá-lo, existem as seguintes maneiras para congelar um objeto:
- **shallow freeze**: `Object.freeze(objeto_que_sera_congelado)`, não impede modificações profundas em objetos aninhados, realizando um congelamento raso
- **deep freeze**: Uma estrategia é desenvolver uma função recursiva aplicando o mesmo congelamento do **shallow freeze**
## Document Object Model (DOM)
DOM é a representação dos dados, dos objetos que compõe a estrutura e o conteúdo de um documento na web, uma página **HTML** por exemplo

O DOM é organizado em uma estrutura de árvore, onde elementos são representados como nós e objetos que podem ser acessados e alterados

Algumas manipulações que conseguimos realizar com javascript são:
- Selecionar elementos pelo id e classe
- Manipular/alterar o conteúdo do documento
- Lidar com eventos
### Seleção de elementos
Podemos selecionar o conteúdo da pagina e salvar em variáveis para futuras interações, alguns comandos para seleção são:
- `document.getElementById`: Seletor de conteúdo pelo id
- `document.getElementsByClassName`: Seletor de conteúdo pela classe
- `document.querySelector`: Seletor genérico de conteúdo, podemos utilizar os seletores do css
### Manipulação/alteração de conteúdo
Após a seleção de um elemento, podemos capturar o seu conteúdo e também alterá-lo, alguns comandos para manipulação e alteração são:
- `element.textContent`: Retorna o conteúdo visível e não visível, também funciona como atribuição
- `element.innerText`: Retorna somente o conteúdo visível, também funciona como atribuição
- `element.innerHTML`: Retorna o HTML como texto
- `element.classList.add` e `element.classList.remove
- `element.classList.toggle`: Se não existir a classe será adicionada, se existir, será removida
- `element.style`: Manipular propriedades do CSS
- `document.createElement`: Cria elemento HTML. Com o elemento criado, podemos add texto com o `textContent` ou adicionar o elemento dentro de outro com `append` e `prepend`
- `element.setAttribute`: Alteração de parâmetros, são passados como chave e valor
### Eventos
Com a seleção de algum elemento, utilizando o `document.querySelector` por exemplo, podemos add a função de `addEventListener` que irá monitorar determinada ação desse elemento. Essa função recebera os seguintes parâmetros:
- Tipo de evento que será monitorado
- Função que será executada, podemos utilizar o parâmetro `event`dentro da função, que permite a obtenção dos dados do evento

```
const button = document.querySelector("button");

button.addEventListener("click", (event) => {

event.preventDefault();
console.log(event);

});
```

Podemos capturar eventos passando diretamente o comportamento que queremos monitorar

```
const button = document.querySelector("button");

button.onclick = (event) => {

event.preventDefault();
console.log(event);

};
```

Porém, essa estrategia considera apenas o último listener criado, enquanto o `addEventListener` executa todos

Alguns eventos importantes para se conhecer são:
- input: Dispara toda vez que o valor do input é alterado
- keydown: Dispara toda vez que uma tecla é pressionada, captura tudo incluindo CTRL, SHIFT
- keypress: Dispara toda vez que uma tecla de caractere é pressionada

<p style="color: red;">REVISAR PROTOTYPE CHAIN</p>

