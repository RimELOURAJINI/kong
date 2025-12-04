# Routage Officiel API – Kong Gateway

| Route Publique | Service | Port | Sécurité |
|----------------|----------|------|-----------|
| /api/products  | quarkus-service | 8080 | JWT + Rate Limit |
| /api/orders    | quarkus-service | 8080 | JWT + Rate Limit |

## Flux

Client → Kong → Quarkus → Kafka → Database
