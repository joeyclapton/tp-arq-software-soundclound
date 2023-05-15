# Separation of Queries and Commands

Para resumir, a interface do VAS consiste em duas partes: um endpoint para servir seu agregado de acordo com as necessidades do BFF, chamado de queries; e endpoints que expõem operações de entidade central, chamadas de commands. Essa separação é a ideia central por trás do padrão CQRS e fornece benefícios práticos, como a possibilidade de fornecer serviços ou armazenamentos separados para operações de leitura e gravação. Essa abstração reduz a complexidade e melhora a consistência nos BFFs.
