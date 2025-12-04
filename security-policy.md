# Politique de Sécurité – API Gateway (Kong)

## 1. Authentification
- Type : JWT (JSON Web Token)
- Header obligatoire :
  Authorization: Bearer <token>

## 2. Rate Limiting
- Limite : 100 requêtes / minute / client

## 3. Plugins actifs
- jwt
- rate-limiting
- prometheus

## 4. Accès
- Toutes les routes /api/* sont protégées
- Accès refusé sans JWT valide

## 5. Monitoring
- Endpoint métriques :
  http://kong:8001/metrics
