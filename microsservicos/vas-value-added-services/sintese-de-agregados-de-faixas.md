# Síntese de Agregados de Faixas

_Value-Added Services (VAS)_ são serviços de negócios responsáveis por retornar uma entidade e seus objetos de valor associados (ou seja, um agregado) ao chamador. É importante notar que um VAS não é responsável por sintetizar metadados para quaisquer entidades associadas. Isso permite uma separação clara de responsabilidades, juntamente com um ponto centralizado onde metadados e regras de autorização para uma determinada entidade podem ser definidos. Um VAS pode orquestrar chamadas a esses serviços para sintetizar e autorizar agregados a serem retornados ao BFF.

Vamos aplicar esses conceitos a exemplos reais da SoundCloud. Uma entidade de exemplo é uma faixa, que tem objetos de valor associados, como metadados, transcodificações e políticas de autorização para determinar a visibilidade. Uma faixa também está conectada a um usuário proprietário, mas como esta é outra entidade, ela contém apenas o ID do usuário como referência. Se um serviço consumidor tiver um ID de faixa que deseja resolver, ele chamará o VAS de Faixas, que se encarrega de garantir que a faixa seja visível e, em seguida, retorna o agregado de faixa correspondente.

Anteriormente, se um usuário final quisesse buscar uma faixa, a solicitação seria enviada ao BFF. Caberia então ao BFF determinar se o usuário da sessão tinha autorização para ver essa faixa e, se sim, sintetizar a representação externa da faixa para retornar ao usuário. Isso envolveria chamadas a vários serviços Foundation que são individualmente responsáveis por retornar informações de autorização e metadados de faixa.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Com a introdução do serviço Tracks VAS, o padrão anterior de chamadas duplicadas aos serviços Foundation nos BFFs foi alterado. Agora, todo o processo de síntese de agregados de faixas para os BFFs é feito pelo serviço Tracks, que também lida com a visibilidade e autorização específicas do contexto das faixas. O BFF agora é responsável por mapear os agregados internos de faixas para representações externas para os clientes consumirem. Isso remove a necessidade de orquestrar chamadas aos serviços Foundation e garante que a autorização seja tratada corretamente.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

**Evolução do VAS no SoundCloud**

No artigo, é explicado como a arquitetura de Value-Added Services (VAS) foi introduzida na SoundCloud para gerenciar entidades e objetos de valor. Em 2019, a equipe de desenvolvimento da SoundCloud implementou um serviço Tracks usando o conceito de VAS, o que ajudou a validar o modelo para outras entidades. Em 2020, a equipe começou a refatorar a API pública, e os endpoints relacionados a faixas foram facilmente migrados para usar o serviço Tracks VAS. No entanto, a equipe precisou decidir se duplicaria a lógica de autorização e busca de listas de reprodução de outros BFFs para a API pública ou criaria um novo serviço de VAS de listas de reprodução. Eles optaram pela última opção, o que exigiu refatorar os outros BFFs para usar o serviço de VAS de listas de reprodução. O gráfico a seguir ilustra o processo de migração:

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

O esquema anterior das APIs da SoundCloud mostrava que elas se conectavam diretamente ao Mothership e à Playlist Authorization, o que gerava dois problemas principais: duplicação de lógica nos BFFs e autorização frágil. O gráfico a seguir mostra nossa solução:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Com esta arquitetura, toda a lógica é centralizada no Playlist VAS.

Este projeto foi dividido nos seguintes passos:

**Extracting the logic:** Extração da lógica dos BFFs e criação de um novo serviço centralizado de playlists (Playlist VAS). A equipe investigou e documentou todas as diferentes implementações da lógica de playlists em seus principais serviços para desenvolver a lógica básica de busca de playlists. Depois de coletar feedback da organização, a equipe implementou o serviço.

**Automatic tests:** Testes automáticos para garantir que a lógica centralizada corresponde aos serviços refatorados. Foram adicionados testes unitários para garantir que todos os possíveis cenários de busca e autorização de playlists fossem cobertos. Também foram adicionados testes de integração para garantir que os formatos de resposta com os clientes não fossem quebrados e que as integrações com serviços dependentes funcionassem corretamente.

**Migrating the BFFs:** Usar o Playlists VAS. Este passo pode parecer fácil, mas na verdade levou mais tempo do que implementar o serviço em si. Tivemos que migrar mais de 50 endpoints relacionados a playlists dos BFFs principais. Fizemos isso ponto a ponto e comparando cuidadosamente as respostas das duas implementações, pois não queríamos quebrar nada nos clientes dos BFFs.

As vantagens da implementação de um serviço de acesso de valor (VAS) para a lógica de resolução de playlists. O VAS centraliza a lógica de negócios e simplifica o desenvolvimento de recursos cruzados em diferentes plataformas. A centralização da lógica de autorização em um único serviço ajuda a evitar inconsistências e vulnerabilidades. A comunicação entre serviços VAS é possível sem dependências circulares. No entanto, a criação de um novo serviço tem custos de manutenção e infraestrutura e pode aumentar as latências da rede. Uma abordagem alternativa seria ter o VAS como uma biblioteca externa, mas isso apresenta riscos de complexidade e versão. O uso do Twinagle simplifica a integração e a manutenção de serviços.
