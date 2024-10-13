## Camada de Domínio

A camada de domínio contém a lógica principal da aplicação e os tipos corporativos essenciais. Esta camada deve ser autossuficiente e não depender de nada externo a ela. Normalmente, é aqui que são definidos os `modelos e estruturas de dados` que representam as entidades e conceitos de negócio.

### Apenas valores de domínio devem existir aqui!

Todo objeto que precise de configurações específicas para uma ferramenta externa (como tags para JSON, BSON, XML, SQL, ou bindings) deve ser mantido fora do domínio, em um projeto separado.

```go
  type UserDomain struct {
    Id       string
    Email    string
    Password string
    Name     string
    Age      int8
  }
```  

### É realmente necessário separar todos os objetos?

Pense no domínio como o `objeto de verdade`, onde estão as informações principais sobre o negócio. Se você adicionar tags de ferramentas externas, como JSON, MongoDB, SQL, ou XML, qualquer alteração feita em uma dessas ferramentas exigiria mudanças no domínio, o que poderia prejudicar a autonomia e estabilidade dessa camada.

### Exemplos de objetos que não pertencem ao domínio

```go
  type Address struct {
    Id       int
    Address1 string         `sql:"not null;unique"`
    Address2 string         `sql:"type:varchar(100);unique"`
    Post     sql.NullString `sql:"not null"`
  }

  type UserRequest struct {
    Email    string `json:"email" binding:"required"`
    Password string `json:"password" binding:"required,min=6,containsany=!@#$%*"`
    Name     string `json:"name" binding:"required,min=4,max=100"`
    Age      int8   `json:"age" binding:"required,min=1,max=140"`
  }

  type UserEntity struct {
    ID       primitive.ObjectID `json:"id" bson:"_id,omitempty"`
    Email    string             `bson:"email,omitempty"`
    Password string             `bson:"password,omitempty"`
    Name     string             `bson:"name,omitempty"`
    Age      int8               `bson:"age,omitempty"`
  }
```  

### E quando um objeto possui muitos campos? Preciso fazer a conversão manual todas as vezes?

Um dos desafios deste design é manter os dados organizados e separados por contexto. Dessa forma, quando houver mudanças no domínio, o impacto será limitado, reduzindo as chances de comprometer o projeto como um todo.

Para facilitar, você pode usar uma biblioteca que simplifique a conversão, como o `copier` do GoLang, que automatiza esse processo.
