# Growing Aggregates

Um VAS pode ser imaginado como um grande "fanout" com lógica de autorização, onde o serviço busca estados de entidades associadas a partir de serviços Foundation correspondentes, e então aplica regras de autorização de negócios. No entanto, o tamanho do fanout pode ser um desafio à medida que os agregados crescem.

Para resolver esse problema, foi utilizado respostas parciais, permitindo que os consumidores da API informem ao produtor qual parte da resposta eles vão consumir, especificando um FieldMask na solicitação. Isso permite que os endpoints centralizados de agregação sejam personalizados para atender às necessidades específicas dos BFFs. Usamos Twinagle, uma IDL protobuf baseada no protocolo Twirp, e as definições protobuf fornecem construção e validação seguras por meio de FieldMaskUtils.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Uma desvantagem das máscaras de campos para respostas parciais é um acoplamento mais rígido entre a topologia dos micros serviços e os esquemas de agregados (IDLs). As máscaras de campo podem ser definidas de acordo com as dependências de serviço e as chamadas de rede para reduzir o número de solicitações necessárias para produzir uma representação do BFF. Na SoundCloud, o foco está mais na redução da complexidade na camada de borda (especificamente nos BFFs). Embora as máscaras de campos possam otimizar as chamadas de rede também, não é necessário ter uma correspondência direta entre as máscaras de campos e as chamadas de rede.
