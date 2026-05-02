
# Objetivo

Integrar diferentes sistemas

Funcionara como uma porta de entrada para os dados vindos de diferentes sistemas

O foco dele é como mecanismo de transporte de dados

# Conceitos do Kafka

- **Topics**: São semelhantes a uma "tabela" em um banco de dados e representam um fluxo particular de dados. Cada tópico possui um nome único que o identifica dentro do Kafka.
- **Partitions**: Um tópico é dividido em várias partições, que são fluxos de dados independentes. Cada mensagem dentro de uma partição é ordenada e recebe um identificador incremental chamado "offset."
- **Offsets**: São identificadores únicos atribuídos a cada mensagem dentro de uma partição, representando a posição da mensagem. Eles ajudam a manter a ordem das mensagens dentro de uma partição, mas não entre diferentes partições.

Ordenamento das mensagem é garantido apenas a nível de partição

Os dados tem uma data de validade e os dados são imutáveis quando escritos nas partições

## Producers

Possuem a função de enviar os dados para os **topics**

As mensagens podem ser enviadas com uma chave, isso irá garantir as mensagens com a mesma chave sejam entregues para a mesma **partition**

Caso a mensagem seja enviada se uma chave, ela pode ser null, a mensagem sera enviada de maneira **round robin**. A mensagem será enviada cada hora para uma partition diferente

### Estrutura das mensagens

![[kafka_message_anatomy.png]]

### Producer Serializer

Maneira como transformamos objetos em bytes, são utilizados para as chaves e os valores

# Boas praticas

Artigo sobre nomeação: [link](https://cnr.sh/posts/2017-08-29-how-paint-bike-shed-kafka-topic-naming-conventions/)
