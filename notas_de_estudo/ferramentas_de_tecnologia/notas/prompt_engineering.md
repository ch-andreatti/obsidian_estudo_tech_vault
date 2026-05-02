Nota com o objetivo de fornecer boas praticas e guias para interagir com LLMs e outras IAs da maneira mais efetiva, com o objetivo de conseguir melhores resultados.

Ao criar um prompt, pense em como você explicaria a tarefa para uma pessoa ao seu lado. Se a explicação for clara para um humano, provavelmente também será para o modelo de linguagem.

# Estrutura básica de prompt (P.R.O.M.P.T.)

- **Persona**: Defina a persona ou papel que o modelo deve assumir ao responder.
- **Roteiro**: Especifique o roteiro ou tarefa que o modelo deve realizar.
- **Objetivo**: Deixe claro qual é o objetivo do prompt e o que se espera alcançar.
- **Modelo**: Indique o formato ou estrutura que a resposta do modelo deve seguir.
- **Panorama**: Forneça informações contextuais relevantes para ajudar o modelo a gerar uma resposta mais precisa e específica.
- **Transformar**: Esteja preparado para refinar e iterar o prompt com base nos resultados obtidos, fornecendo feedback e exemplos adicionais se necessário.

# Técnicas

## Técnicas aplicadas diretamente no chat
### Zero-Shot Prompt
É a técnica onde um modelo é solicitado a executar uma tarefa sem exemplos prévios. O prompt fornece apenas a **instrução direta** do que se espera como resposta.

**Quando usar:**  
- Quando a tarefa é simples, direta ou rotineira.
- Quando o modelo já foi treinado amplamente sobre o domínio da tarefa.
- Quando se deseja economizar tokens.

**Exemplo de uso:**
```
Traduza o seguinte texto para o francês: "Olá, como vai você?"
```

**Situações ideais:**  
- Tradução simples
- Geração de títulos
- Resumos curtos
- Perguntas objetivas

### Few-Shot Prompt
Inclui **poucos exemplos de entrada e saída** no prompt para ensinar o modelo a realizar a tarefa. Os exemplos ajudam a guiar a estrutura e o estilo esperados.

**Quando usar:**
- Quando a tarefa é mais complexa ou ambígua
- Quando se deseja replicar um estilo específico
- Quando há múltiplas formas válidas de resolver a tarefa

**Exemplo de uso:**
```
Corrija os erros gramaticais:

Entrada: "Eu vai na escola todos os dias."
Saída: "Eu vou à escola todos os dias."

Entrada: "Ela comeram arroz e feijão."
Saída:
```

**Situações ideais:**
- Reescrita de texto com estilo definido
- Classificação personalizada
- Geração de conteúdo seguindo padrões específicos

### Chain of Thought (CoT)
Solicita que o modelo **explique ou raciocine passo a passo** antes de dar a resposta final. Ideal para tarefas que exigem **lógica, matemática ou inferência**.

**Quando usar:**
- Quando a tarefa envolve múltiplas etapas lógicas
- Quando a precisão é importante
- Quando se quer interpretar como o modelo “pensa”

**Exemplo de uso:**
```
Quantas laranjas Pedro terá se ele tem 3 cestos com 4 laranjas em cada um?

Explique seu raciocínio passo a passo antes de responder.
```

**Situações ideais:**
- Problemas matemáticos
- Questões de lógica
- Diagnóstico de falhas
- Planejamento de estratégias

### Context, Instructions, Details, Input (CICD)
É uma técnica que estrutura o prompt em 4 partes:
1. **Context**: Explica a situação ou o papel do modelo
2. **Instructions**: O que o modelo deve fazer
3. **Details**: Regras, formatos, limitações
4. **Input**: O dado de entrada específico

**Quando usar:**
- Para tarefas mais longas ou multifásicas
- Quando o controle do formato e da qualidade da resposta é crucial
- Em fluxos automatizados ou integrações com sistemas

**Exemplo de uso:**
```
Context: Você é um revisor gramatical especializado em português formal.

Instructions: Reescreva o texto mantendo o significado, mas aplicando a norma culta da língua.

Details: A resposta deve ter no máximo 100 palavras. Mantenha o estilo neutro e objetivo.

Input: "A gente tamo tentando resolver os problema, mas tá difícil."
```

**Situações ideais:**
- Aplicações profissionais (jurídico, saúde, dados)
- Geração de relatórios
- Geração de código
- Uso em pipelines de automação

## Técnicas aplicadas programaticamente
- Context, Instructions, Details, Input (CICD)
- Tree of Thoughts
- Retrieval Augmented Generation
- ReAct Prompt

# Evitando alucinações
## Permitir que o modelo diga que não sabe a resposta
Ao dar permissão explícita para o modelo admitir quando não tem conhecimento suficiente para responder uma pergunta, você pode reduzir a quantidade de informações fabricadas ou imprecisas nas respostas.
## Pedir para o modelo encontrar evidências antes de responder
Instrua o modelo a procurar trechos relevantes no contexto fornecido que embasem sua resposta. Você pode usar tags especiais, como `<Quotes>`, para indicar onde o modelo deve citar as evidências encontradas.
## Permitir que o modelo "pense" antes de responder
Dê espaço para o modelo refletir e processar a informação antes de gerar uma resposta.
## Temperatura do Modelo
Ajuste a temperatura do modelo para controlar a criatividade. Utilize valores baixos para respostas mais factuais.

Para complementar assistir o video:
Engenharia de Prompt: O Guia Definitivo