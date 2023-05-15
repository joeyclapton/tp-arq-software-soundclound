# Separation of Queries and Commands

Podemos notar a necessidade de criar uma abordagem escalável para gerenciar entidades em diferentes domínios de negócios. Em vez de implementar tudo o que pode ser feito com uma entidade (como uma música), em todos os domínios, é recomendado identificar os diferentes domínios de negócios que precisam usar a mesma entidade e criar um Gateway de Domínio para cada um deles. O Gateway de Domínio é uma implementação de um Serviço de Valor Agregado (VAS) ligado a um domínio específico. Cada Gateway pode ser mantido por equipes diferentes e representar visões diferentes sobre a mesma entidade, utilizando a mesma camada fundamental de serviços. A abordagem de Gateway de Domínio oferece escalabilidade, autonomia e estabilidade para cada um dos domínios.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
