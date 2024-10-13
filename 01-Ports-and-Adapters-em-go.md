## O que é Arquitetura Hexagonal?

Também conhecida como `Ports and Adapters`, a arquitetura hexagonal é uma forma de organizar o código em camadas, onde cada camada possui uma responsabilidade específica. O principal objetivo é isolar a lógica da aplicação do mundo externo. Esse isolamento é realizado através de Portas e Adaptadores, onde as `Portas` representam interfaces que as camadas de baixo nível expõem, enquanto os `Adaptadores` são implementações dessas interfaces.

### Adaptadores - Input

A camada de adaptadores é dividida em duas partes, sendo uma delas a de input. Todas as formas de entrada de dados da aplicação devem estar nesta camada, unificando-se em um adaptador de entrada.

* `controllers/consumers/listeners` são exemplos de formas de entrada de dados que podem existir nesta camada de input.  
* `model para entrada de dados` organiza as requisições e respostas para APIs, filas, etc., separando-as em:
  * Model/request  
  * Model/response  
* `mappers/converters` mapeiam o modelo de request/response para o modelo de domínio (camada de domínio) e vice-versa.  
* `validations (opcional)` é onde podem ser realizadas validações dos objetos de entrada antes de sua conversão para a camada de domínio.

### Adaptadores - Output

A outra parte dos adaptadores é dedicada ao output. Todas as formas de saída de dados da aplicação devem ser organizadas nesta camada, unificando-se em um adaptador de saída.

* `restClient/grpcClient/producers/repositories` são formas de saída de dados nesta camada de output.  
* `model para saída de dados` organiza as saídas para APIs, filas, entidades de banco de dados, etc., dividindo-as em:
  * Model/request  
  * Model/response  
* `mappers/converters` mapeiam os modelos de request/response de saída para o modelo de domínio e vice-versa.  
* Além dos clients, `podemos ter clientServices`, que realizam enriquecimento de informações antes de chamar a camada externa.

### Application

A camada `application` é o núcleo da aplicação. Nela, encontram-se todas as regras de negócio nos `services`, e, mais importante, a camada `domain`, que contém o objeto central da aplicação.

* `services` é onde a lógica principal da aplicação é validada e processada.  
* `useCases/Ports` oferece acesso às interfaces de entrada e saída.  
* `domain` define o objeto fundamental da aplicação, onde todas as informações relevantes para o fluxo em execução são mantidas.

### Configurações

Como toda aplicação, também é necessário gerenciar as configurações relativas a acessos externos, ambientes, feature-toggles, entre outros.

`A nomenclatura das configurações não segue um padrão único,` mas é ideal que os nomes reflitam claramente o propósito da configuração, como nos exemplos a seguir:

- Repository: adapter/output/database/mongodb  
  Configuration: database/mongodb  
- HttpTestMS: adapter/output/http/testMs  
  Configuration: http/testMs  

### Quando (e por que) usar a Arquitetura Hexagonal?

A escolha pela arquitetura hexagonal deve ser criteriosa, pois não é um modelo simples de implementar. Para projetos menores que não passarão por mudanças frequentes, pode ser mais prático optar por uma arquitetura mais direta.

`Todas as arquiteturas funcionam` bem, contanto que sejam aplicadas corretamente e de acordo com a complexidade do projeto.
