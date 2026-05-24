# Parquet
É um formato de armazenamento colunar open-source, criado pelo Twitter e Cloudera em 2013, amplamente adotado no ecossistema Hadoop/Spark e em soluções de Data Lake modernas. Suas principais características são:
- Armazenamento hibrido de dados (linha e colunar)
- Compreensão eficiente
- Schema embutido (tipos de dados, nomes de colunas, metadados)
- 

![arquivos_de_suporte/taxa_de_compactacao_do_parquet.png](../arquivos_de_suporte/taxa_de_compactacao_do_parquet.png)

Encodings Utilizados:
- Plain
- RLE (Run-Length Encoding)
- RLE_DICTIONARY (Dictionary Encoding)
- BIT_PACKING
## Encoding - Run length encoding

Compressão do tipo lossless

Segundo o vídeo, essa é a mais importante

Ordenação é crucial nesse aspecto
Dados devem ser ordenados da menor cardinalidade para maior
Fazemos ordenação na escrita

Tipo de arquivo colunar, minimizamos colunas lidas
Encoding, minimizamos quantidade de dados lidos
Partição, minimizamos quantidade de linhas lidas


# Delta Lake

## Características
- Open source
- Transação ACID
- Merge
- Schema
- Auditoria e versionamento

Principal resolução para Lakehouse

## Otimização

### OPTIMIZE
Melhora a performance de leitura, pois compacta **pequenos arquivos** em **arquivos maiores**, reduzindo a quantidade de arquivos. Isso reduz o custo de leitura, já que menos arquivos precisam ser abertos durante um scan

Ajuda a prevenir o problema de **small files**

### VACUUM
É responsável por **remover fisicamente os arquivos que não são mais referenciados** no log de transações da tabela (arquivos "órfãos", antigos ou obsoletos).

Esses arquivos surgem quando você faz operações como:
- `UPDATE`
- `DELETE`
- `MERGE`
- `OVERWRITE`

O Spark não apaga imediatamente os arquivos antigos, para permitir funcionalidades como **Time Travel** e **consistência de leitura entre leitores e escritores**.

Podemos utilizar o parâmetro `RETAIN` para especificar quais arquivos queremos manter. Exemplo: `VACUUM minha_tabela RETAIN 24 HOURS`
