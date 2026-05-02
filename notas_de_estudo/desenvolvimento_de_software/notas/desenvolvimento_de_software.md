# Nomenclatura de código
## Sempre usar uma palavra por conceito
A fim de manter consistência e tornar o código mais fácil de se entender, devemos definir uma padrão sobre as palavras que representam determinado conceito ou ação, exemplos:

```python
# ❌ Errado

GetUser()  
FetchOrder()  
RetrieveProduct()  
LoadInvoice()  
ObtainCustomer()

# Essas classes fazem a mesma tarefa, de retornar dados

# ✅ Correto

GetUser()  
GetOrder()  
GetProduct()  
GetInvoice()  
GetCustomer()

# Poderia ser Fetch, Load, ...
# O importante, é usar apenas uma palavra
```

```python
# ❌ Errado

OrderProcessor  
PaymentHandler  
ShipmentManager  
InvoiceController  
CustomerCoordinator

# Serviços que aplicam lógica de negócio

# ✅ Correto

OrderService  
PaymentService  
ShipmentService  
InvoiceService  
CustomerService
```

# Versionamento semântico
Padrão de versionamento de software, possui a seguinte estrutura: 
`9 (Major) . 1 (Minor) . 3 (Patch)`

**Major (Versão principal):**  Aumenta quando as alterações irão causar conflitos com as versões anteriores. Isso significa que, se você atualizar para uma nova versão principal, provavelmente haverá alterações que quebrarão a compatibilidade com a versão anterior, causando a necessidade de refatoração.

**Minor (Versão menor):** Aumenta quando são adicionadas novas funcionalidades de maneira compatível com versões anteriores. Essa atualização não devem introduzir alterações que quebram a compatibilidade com o código existente.

**Patch (Versão de correção):** Aumenta quando correções de bugs compatíveis com versões anteriores são adicionadas. Essas correções não devem introduzir novas funcionalidades ou quebrar a compatibilidade com o código existente.


# Build, artefatos e release

## Build
É o processo de **compilar, empacotar e preparar o código-fonte** para ser executado ou distribuído.  Pense nele como “transformar o código-fonte cru em algo executável”. Exemplos:
- Em **Python**: gerar um pacote `.whl` ou `.zip` com as dependências corretas
- Em **Java**: compilar os `.java` em `.class` e empacotar em um `.jar` ou `.war`
- Em **front-end (React, Angular)**: gerar os arquivos minificados de HTML/CSS/JS prontos para o servidor

Uma analogia para esta etapa, pensando em um restaurante, seria em pegar os ingredientes (código) e preparar um prato (artefato)

## Artefatos
É qualquer arquivo gerado como resultado do processo de **build**. Exemplos:
- `.whl` (no caso de Python)
- `.jar` (no caso de Java)
- `.exe` (em C#)

## Release
É o processo de **distribuir o build validado** para um ambiente (teste, homologação, produção).  É quando o artefato produzido no **build** é **implantado (deploy)** para uso real. Exemplos:
- Publicar uma biblioteca no **PyPI**
- Fazer deploy de uma API no **Azure App Service**
- Enviar uma nova versão de aplicativo para a **Play Store**

Uma analogia para esta etapa, pensando em um restaurante, seria em entrega o prato pronto para o cliente (usuário final)
