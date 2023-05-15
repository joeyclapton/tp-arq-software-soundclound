# Commands

\
Enquanto estendíamos o escopo do VAS para servir agregados das nossas entidades, identificamos que poderíamos também estender o VAS para ações que mutam o estado da entidade principal, mas ao mesmo tempo exigem lógica de autorização. Para centralizar ainda mais as entidades principais, estendemos nosso VAS com comandos. Alguns exemplos dessas operações de comando no domínio de Faixas incluem "baixar uma faixa", "curtir uma faixa" e "repostar uma faixa".

Uma vez que é uma operação que vive no VAS, também tem a vantagem de reduzir a lógica complexa nos BFFs (no caso de tal lógica ter sido duplicada lá) e melhorar a confiabilidade em termos de lógica de acesso daqueles comandos que exigem acesso concedido a uma determinada faixa. Podemos ilustrar o caso de curtir uma faixa na VAS de faixas:

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Como podemos ver no gráfico, os BFFs enviariam uma solicitação para o serviço de Tracks para executar uma operação de faixa. O serviço que geralmente registra operações "curtir" fica no serviço Likes. Este serviço não está ciente de autorizações de faixas; ele apenas cria/exclui links entre faixas/playlists e usuários. É por isso que precisamos verificar primeiro se o usuário que deseja curtir uma faixa tem acesso a ela. O melhor lugar para realizar essa lógica de forma centralizada é no Tracks VAS.
