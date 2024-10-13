## Camada Application

### Como ela funciona?

A entrada de usuários ou sistemas externos chega ao `core/application` por meio de uma porta, usando um adaptador, e permite que a saída seja enviada ao `core/application` por uma porta para um adaptador correspondente. Isso cria uma camada de abstração que protege o núcleo da aplicação, `isolando-o de ferramentas e tecnologias externas` que são irrelevantes para o funcionamento interno.

### Trabalhe apenas com regras internas e sem dependências externas

Na camada application, foque em `receber dados já transformados no formato de Domain`. Se precisar acessar dados externos para complementar a lógica, utilize as `portas de saída` para buscar essas informações.

### Exemplos de dados externos para complementar regras de negócio

Um exemplo simples é a criação de um usuário, onde é necessário validar se o e-mail já existe no banco de dados. Para isso, é recomendável `utilizar um Use Case, receber o objeto de domínio como retorno e aplicar a regra`:

```go
  user, _ := ud.FindUserByEmailServices(userDomain.Email) // Acesso a dados externos
  if user != nil {
    return nil,
      rest_erros.NewBadRequestError(
        message: "Email is already registered in another account")
  }

  userDomain.EncryptPassword()
  userDomainRepository, err := ud.repository.CreateUser(userDomain) // Acesso a dados externos
  if err != nil {
    logger.Error(message: "Error trying to call repository", err,
    zap.String(key: "journey", val: "createUser"))

    return nil, err
  }
``` 

### Onde ficam as interfaces de entrada e saída?

Você pode se perguntar onde estão localizadas as interfaces de entrada e saída. Como a camada de application `não deve importar elementos externos`, é necessário definir as `interfaces de entrada (ports)` e `interfaces de saída (use cases)` dentro dela:

application/  
├── constantes/  
├── domain/  
└── port/  
&nbsp;&nbsp;&nbsp;├── input/  
&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;└── user_use_case.go  
&nbsp;&nbsp;&nbsp;└── output/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── user_port.go  

Quando alguém externo chama o Service - `Use Case`.  
Quando o Service chama alguém externo - `Port`.
