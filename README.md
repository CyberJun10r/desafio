# desafio
catálogo de produtos
Sistema de Catálogo de Produtos e Simulação de Pedidos com Microsserviços
Este documento descreve a arquitetura e o funcionamento de um sistema simples baseado em microsserviços para gerenciamento de catálogo de produtos e simulação de pedidos. O sistema é desenvolvido com Spring Boot e Spring Cloud, utilizando API Gateway e Service Discovery via Eureka.

Visão Geral
O sistema é composto por:
- Microsserviço de Catálogo: responsável por gerenciar produtos.
- Microsserviço de Pedidos: responsável por criar e simular pedidos com base nos produtos do catálogo.
- Eureka Server: atua como repositório de registro e descoberta dos serviços.
- Spring Cloud Gateway: direciona as requisições para os serviços corretos.

Microsserviços
Microsserviço: Catálogo de Produtos
Responsabilidades:
- Criar, listar, atualizar e deletar produtos.
- Fornecer detalhes de produtos para o serviço de pedidos.
Endpoints Exemplo:
- GET /products – Lista todos os produtos.
- GET /products/{id} – Detalhes de um produto.
- POST /products – Criação de novo produto.
- PUT /products/{id} – Atualização de produto.
- DELETE /products/{id} – Remoção de produto.

Microsserviço: Simulação de Pedidos
Responsabilidades:
- Criar e listar pedidos.
- Calcular valores com base nos produtos selecionados.
- Consultar detalhes dos pedidos realizados.
Endpoints Exemplo:
- POST /orders – Criação de pedido com IDs de produtos.
- GET /orders – Listagem de pedidos.
- GET /orders/{id} – Detalhes de um pedido específico.
O serviço de pedidos comunica-se com o serviço de catálogo via HTTP utilizando WebClient ou Feign.

Comunicação entre Serviços
- Registro no Eureka: Cada microsserviço e o API Gateway se registra automaticamente no Eureka Server.
- Descoberta dinâmica: os serviços se comunicam com nomes simbólicos definidos no Eureka, sem necessidade de IPs fixos.
- Chamadas entre microsserviços: o microsserviço de pedidos utiliza chamadas HTTP para buscar dados de produtos no catálogo.

API Gateway
O Spring Cloud Gateway intercepta as requisições dos clientes e direciona com base em padrões de rota:
- /api/products/** → serviço de catálogo
- /api/orders/** → serviço de pedidos
Ele também serve como ponto de entrada único para o cliente externo.

Fluxo de Requisição - Cenário: Criação de Pedido
- O cliente envia um POST /api/orders com os IDs dos produtos.
- O API Gateway intercepta e redireciona para o serviço de pedidos.
- O serviço de pedidos consulta o serviço de catálogo para obter os detalhes dos produtos.
- Calcula o valor total e armazena o pedido.
- Retorna a confirmação ao cliente.

Ferramentas e Dependências Recomendadas
Adicione as seguintes dependências no pom.xml:
- spring-boot-starter-web
- spring-cloud-starter-netflix-eureka-server
- spring-cloud-starter-netflix-eureka-client
- spring-cloud-starter-gateway
- spring-cloud-starter-openfeign (opcional)
- spring-boot-starter-actuator

Considerações Finais
- Utilize DTOs para transferência de dados entre serviços.
- Faça uso de tratamento de falhas (como circuit breakers) com Resilience4j ou Hystrix.
- Centralize logs para facilitar rastreabilidade.
- Mantenha os microsserviços independentes e coesos.
