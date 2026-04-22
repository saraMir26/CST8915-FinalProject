## Architecture Diagram

```mermaid
graph TD

User -->|HTTP| StoreFront[Store-Front Service]
User -->|HTTP| StoreAdmin[Store-Admin Service]

StoreFront -->|GET /products| ProductService[Product-Service]
StoreFront -->|POST /order| OrderService[Order-Service]

StoreAdmin -->|GET /orders| MakelineService[Makeline-Service]
StoreAdmin -->|Process Order| MakelineService

OrderService --> MongoDB[(MongoDB StatefulSet)]
ProductService --> MongoDB
MakelineService --> MongoDB

subgraph AKS Cluster
    StoreFront
    StoreAdmin
    OrderService
    ProductService
    MakelineService
    MongoDB
end
