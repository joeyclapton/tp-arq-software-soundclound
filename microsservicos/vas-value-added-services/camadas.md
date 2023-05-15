# Camadas

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

**Edge:** Esta camada fornece capacidades de gateway de API e é onde nossos BFFs (backends for frontends) vivem. Os BFFs são APIs dedicadas publicadas e mantidas que são adaptadas às necessidades específicas do cliente.

**Value Added:** Serviços nesta camada consomem dados de outros serviços e os processam de alguma maneira para construir experiências ricas para os usuários.

**Foundation:** Este é um serviço de baixo nível que fornece os blocos de construção em torno de um domínio.

Também é importante entender os blocos de construção que se unem nos Serviços com Valor Agregado.&#x20;



\
**Domain**: Uma preocupação do usuário ou do negócio que pode ser usada para definir limites/escopo em torno das integrações de serviço.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

**Entity**: Uma entidade é um objeto que possui um identificador independente e um ciclo de vida.&#x20;

**Value Objects**:  Os objetos de valor contêm metadados relacionados a uma determinada entidade; eles também estão ligados ao ciclo de vida da entidade.

&#x20;**Aggregate**: Um agregado é uma coleção de uma ou mais entidades relacionadas. Um agregado possui uma entidade raiz chamada raiz do agregado. Os agregados também podem conter referências a outras entidades, mas não aos metadados da entidade referenciada. Então, cabe aos serviços consumidores chamar outros serviços para sintetizar as referências da entidade.
