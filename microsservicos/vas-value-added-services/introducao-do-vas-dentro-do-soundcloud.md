# Introdução do VAS dentro do SoundCloud

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
