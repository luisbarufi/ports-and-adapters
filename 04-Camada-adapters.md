## Camada adapters

### Como ela funciona?

O `core` da aplicação utiliza `interfaces dedicadas, chamadas "portas"`, para se comunicar com o mundo externo. Essas portas permitem tanto a `entrada quanto a saída de dados` para a aplicação.

No cenário de input, os dados externos chegam na aplicação e precisam ser comunicados de alguma forma. Para isso, usamos `interfaces de entrada`, que padronizam a forma de comunicação. Todo dado de entrada deve seguir a mesma estrutura:  


HTPP Endpoint&nbsp;------------>&nbsp;|  
SQS&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;---------------------->&nbsp;|  
Kafka&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-------------------->&nbsp;|&nbsp;&nbsp;-> `func createUser(user.domain.User) {}`  
gRPC&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--------------------->&nbsp;|  
CLI&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;---------------------->&nbsp;|  
Outro&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-------------------->&nbsp;|  



### Diante disso, todo e qualquer dado externo tem que ser convertido em domain!  

Mas como nossa regra, ou core, da aplicação fica agnostico à origem do dado? Simple, `domain`  


HTPP Endpoint&nbsp;------------>&nbsp;userRequestHTTP&nbsp;&nbsp;&nbsp;|  
SQS&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;---------------------->&nbsp;userRequestSQS&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|  
Kafka&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-------------------->&nbsp;userRequestKafka&nbsp;&nbsp;|&nbsp;&nbsp;-> Converter -----------> `UserDomain -> Service`  
gRPC&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--------------------->&nbsp;userRequestgRPC&nbsp;&nbsp;&nbsp;|  
CLI&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;---------------------->&nbsp;userRequestCLI&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|  
Outro&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-------------------->&nbsp;userRequestOther&nbsp;|  
