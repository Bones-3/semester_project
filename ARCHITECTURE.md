# Architecture Overview

This repository is organized as a Spring Cloud microservices architecture. The main entry point is the API Gateway, which routes client requests to the appropriate service. Eureka provides service discovery, allowing services to register and discover one another without hardcoded endpoints.

```mermaid
flowchart LR
    User[Client / Browser / Mobile App]
    Gateway[ms_gateway\nAPI Gateway]
    Eureka[ms_eureka\nService Registry]
    Auth[ms_autentificacion\nAuthentication Service]
    Users[ms-usuarios\nUser Service]
    Products[ms-productos\nProduct Service]
    Orders[ms-pedidos\nOrder Service]

    User -->|HTTP / REST| Gateway
    Gateway -->|Discover services| Eureka

    Gateway -->|Login / tokens| Auth
    Gateway -->|User management| Users
    Gateway -->|Product catalog| Products
    Gateway -->|Orders & payments flow| Orders

    Auth -->|Registers with| Eureka
    Users -->|Registers with| Eureka
    Products -->|Registers with| Eureka
    Orders -->|Registers with| Eureka

    Auth --> DBAuth[(Auth Database)]
    Users --> DBUsers[(Users Database)]
    Products --> DBProducts[(Products Database)]
    Orders --> DBOrders[(Orders Database)]
```

## Service responsibilities

- `ms_gateway`: central API entry point and request routing
- `ms_eureka`: service registry and discovery
- `ms_autentificacion`: authentication and authorization logic
- `ms-usuarios`: user profile and account-related operations
- `ms-productos`: catalog and product management
- `ms-pedidos`: order creation and order processing

This design provides independent deployment, scalability, and fault isolation for each business capability.