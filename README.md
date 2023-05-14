# Arquitetura SoundCloud

## Introdução

> O SoundCloud é uma plataforma global de áudio e música fundada na Suécia que permite que seus usuários carreguem, promovam e transmitam áudio com vários serviços de assinatura.

* Como uma plataforma aberta que conecta diretamente artistas e seus fãs, tornou-se conhecida por seu conteúdo exclusivo e recursos exclusivos conquistando mais de 175 milhões de usuários mensais.
* O site da Sound Cloud usa design adaptável da Web com serviço dinâmico da Web e sua API permite que programas de terceiros carreguem arquivos de áudio ou baixem arquivos com permissão do uploader.
* Isso suporta uma ampla variedade de arquivos, como AIFF, WAV, MP2, MP3 e os transcodifica para mp3 a 128 kbit/s para streaming.
* Ele foi inicialmente construído usando arquitetura monolítica e foi um dos primeiros aplicativos a passar da arquitetura monolítica para a arquitetura de microsserviços.

## Principais Requisitos

1. **Escalabilidade** -> O SoundCloud precisa lidar com uma grande quantidade de uploads, transmissões e interações dos usuários. Portanto, a arquitetura deve ser escalável para suportar o aumento do número de usuários e o crescimento contínuo da plataforma. Para suprir essa demanda, foi adotada uma <mark style="background-color:yellow;">arquitetura distribuída baseada em microsserviços</mark>. Sendo, cada microsserviço responsável por uma função específica. Isso permite que a plataforma dimensione horizontalmente, adicionando mais instâncias dos microsserviços conforme necessário, garantindo a capacidade de lidar com o aumento do tráfego e a demanda dos usuários.
2. **Confiabilidade** -> É fundamental que o SoundCloud seja confiável e esteja disponível para os usuários. O sistema deve lidar com falhas de forma resiliente e garantir uma experiência contínua mesmo em situações adversas. Para alcançar esse objetivo, foi escolhido utilizar a replicação de dados e o balanceamento de carga. Os dados são replicados em vários servidores para garantir a disponibilidade mesmo em caso de falhas. Além disso, a plataforma utiliza o balanceamento de carga para distribuir o tráfego entre os servidores, evitando sobrecargas e garantindo a estabilidade do sistema.
3. **Desempenho** -> Para fornecer streaming de áudio de alta qualidade e permitir uma experiência suave aos usuários, a arquitetura do SoundCloud é projetada para ter um desempenho rápido e eficiente. A plataforma faz uso de técnicas como caching e streaming sob demanda. As músicas mais populares e frequentemente acessadas são armazenadas em cache, reduzindo a latência e permitindo um acesso mais rápido. Além disso, são utilizadas técnicas de compressão de áudio para transmitir músicas com a menor largura de banda possível, garantindo um desempenho otimizado.
4. **Segurança** -> Como plataforma de compartilhamento de conteúdo, o SoundCloud deve proteger os dados dos usuários e garantir a segurança das contas. A arquitetura inclui práticas de segurança, como autenticação de usuário, criptografia de dados e auditorias regulares. São adotadas medidas de autenticação de dois fatores e suporte a autenticação de serviços externos, como o Google e o Facebook, para reforçar a segurança das contas dos usuários. Além disso, são implementados firewalls e sistemas de detecção de intrusões para prevenir ataques e garantir a integridade e a privacidade dos dados dos usuários.



## Arquitetura

### Movendo -se do Monólito para Micro-serviços&#x20;

* O SoundCloud foi criado inicialmente como um único aplicativo monolítico de Ruby no Rails em execução na ressonância magnética, o intérprete oficial de Ruby, e apoiado por Memcached & MySQL.&#x20;
* Com o rápido aumento dos usuários, eles enfrentaram problemas de escala como uma grande rede social com uma potência de distribuição de mídia.&#x20;
* Como solução, eles decidiram avançar em direção à arquitetura de micro-serviços com a ajuda do conceito do "Contexto Delimitado", introduzido por Eric Evans no livro "Domain-Driven Design" e expandido por Martin Fowler em suas palestras e escritos relacionados à arquitetura de software.
* Ao dissociar os serviços de seu aplicativo monolítico de Ruby on Rails chamado _Mothership_, eles moveram com sucesso sua aplicação, do monólito para a arquitetura de micro-serviços.

### Microservices&#x20;
