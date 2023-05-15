---
description: Movendo-se do Monólito para Micro-serviços
---

# Arquitetura

* O SoundCloud foi criado inicialmente como um único aplicativo monolítico de Ruby no Rails em execução na ressonância magnética, o intérprete oficial de Ruby, e apoiado por _Memcached_ & MySQL.&#x20;
* Com o rápido aumento dos usuários, eles enfrentaram problemas de escala como uma grande rede social com uma potência de distribuição de mídia.&#x20;
* Como solução, eles decidiram avançar em direção à arquitetura de micro-serviços com a ajuda do conceito do "Contexto Delimitado", introduzido por Eric Evans no livro "_Domain-Driven Design_" e expandido por Martin Fowler em suas palestras e escritos relacionados à arquitetura de software.
* Ao dissociar os serviços de seu aplicativo monolítico de Ruby on Rails chamado _Mothership_, eles moveram com sucesso sua aplicação, do monólito para a arquitetura de micro-serviços.
