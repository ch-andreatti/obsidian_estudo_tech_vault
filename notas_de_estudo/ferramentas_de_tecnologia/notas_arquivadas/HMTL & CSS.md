
**Data do arquivamento:** 11/08/2025

# Geral

## Comportamentos
Suponha que cada a página é composta por várias linhas, igual a uma folha de caderno
- Block: Uma caixa irá ocupar a linha toda
- Inline: Podem existir múltiplas caixas na mesma linha
# HTML
## Estrutura
- Definição do documento com doctype
- Head: Parte destinada para configuração
- Body: Parte destinada para o conteúdo da página, um exemplo de estruturação do body é o seguinte:
	- Header: Cabeçalho da página
	- Main: Conteúdo principal da página
	- Aside: Conteúdo secundário da página relacionado com o Main
	- Footer: Rodapé, conteúdo de navegação, contato, ...

Algumas tags para estruturar o HTML são:
- `section:` Criar seções, use quando você precisa representar uma seção lógica dentro do seu documento
- `div:` Para criar blocos, possui comportamento **block**, use quando você precisa de um contêiner genérico para aplicar estilos
- `spam:` Mesmo finalidade da div, porém, possui o comportamento **inline**
## Box model
Todos os elementos do HTML são considerados como caixas e irão possuir as seguintes propriedades:
- Conteúdo
- Largura
- Altura
- Borda
- Preenchimento (Espaço interno), padding. O comportamento border-box evita vazamentos por não aumentar a largura da caixa
- Espaçamento (Espaço externo), margin
## Boas praticas
- Seguir a semântica: Cada tag no HTML tem um proposito/objetivo de utilização, semântica é a prática de atribuir significado estruturado aos elementos HTML para melhorar a compreensão do conteúdo pelos navegadores e motores de busca
- O ideal é ter um H1 por documento, que será p tema principal da página

# CSS

## Especificidade
Cada seletor possui uma pontuação, e a declaração com maior quantidade de pontos será aplicada. Cada seletor possui as seguintes pontuações:
- `#id`: 100
- `.class`: 10 
- `element`: 1

Esses elemento podem ser combinados para aumentar a sua pontuação
## Seletores
Seletores são os elementos que especificamos aonde o css deve ser aplicado, existem os seguintes tipos de seletores:
- tag - É utiliza passando apenas a tag desejada
- id - É como se fosse um RG, ter apenas um. Modelo: "# + nome-do-id"
- class - Modelo: ". + nome-da-classe"
- atributo - Modelo: "[]" passar o nome do atributo ou nome + valor do atributo
- universal - Modelo: *
## Combinators
Tipos de combinações existentes para definir aonde o css deve ser aplicado:
- Descendant: Caminho de tags, do nível superior até o mais inferior
- List: Lista de tags separadas por virgula
- Next sibling: Compara a onde o estilo será aplicado e a tag anterior, possui o seguinte modelo: tag-anterior + tag-onde-deve-ser-aplicado, se essa regra for respeita o estilo será aplicado
- Child: Aplicar as primeiro filhos, modelo: tag-pai > tag-filho-nivel-1
### Site para fontes
- [fonts.google](https://fonts.google.com/)
