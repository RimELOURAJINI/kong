# Kong API Gateway – Projet Complet

Ce projet est une stack Docker complète pour déployer un **API Gateway** sécurisé avec **Kong**, gérer les APIs via **Konga**, et stocker les données de configuration avec **PostgreSQL**. Il inclut la configuration déclarative de Kong, la documentation du routage API et la politique de sécurité.

---

## Structure du projet

kongProject/
├── api-routing.md # Documentation des routes API
├── docker-compose.yml # Stack Docker (Kong, Konga, PostgreSQL)
├── kong.yaml # Configuration déclarative de Kong
├── security-policy.md # Politique de sécurité et plugins
├── README.md # Ce document
└── Captures d’écran # Images de l'interface et configuration


---

## Contenu des fichiers

### 1. `docker-compose.yml`

Ce fichier décrit la stack Docker complète :

- **Kong Gateway** :
  - Mode **DB-less**
  - Configuration déclarative via `kong.yaml`
  - Ports exposés : 8000 (Proxy), 8443 (Proxy HTTPS), 8001 (Admin API), 8444 (Admin HTTPS)

- **Konga** (interface de gestion de Kong) :
  - Dépend de la base PostgreSQL et de Kong
  - Port exposé : 1337

- **PostgreSQL** :
  - Stocke la base de données Konga
  - Volume persistant pour sauvegarde

- **Réseau** :
  - `kong-net` pour communication entre les services

---

### 2. `kong.yaml`

Configuration déclarative de Kong :

- **Consumers** :
  - `fleet-client`

- **JWT Secrets** :
  - Key : `fleet-key`
  - Secret : `fleet-secret-123`
  - Algorithme : HS256

- **Services** :
  - `quarkus-service` → backend Quarkus (http://quarkus:8080)

- **Routes** :
  - `/api/products`
  - `/api/orders`

- **Plugins actifs** :
  - `jwt` : authentification
  - `rate-limiting` : 100 requêtes/minute
  - `prometheus` : monitoring

---

### 3. `api-routing.md`

Documentation des routes API exposées par Kong :

| Route | Service | Port | Sécurité |
|-------|---------|------|----------|
| `/api/products` | quarkus-service | 8080 | JWT + Rate Limit |
| `/api/orders`   | quarkus-service | 8080 | JWT + Rate Limit |

**Flux** :  
Client → Kong → Quarkus → Kafka → Database

---

### 4. `security-policy.md`

Décrit les politiques de sécurité :

1. **Authentification**
   - JWT obligatoire
   - Header : `Authorization: Bearer <token>`

2. **Rate Limiting**
   - Limite : 100 requêtes / minute / client

3. **Plugins actifs**
   - `jwt`, `rate-limiting`, `prometheus`

4. **Accès**
   - Toutes les routes `/api/*` sont protégées
   - Rejet sans JWT valide

5. **Monitoring**
   - Endpoint métriques : `http://kong:8001/metrics`

---

## Lancer le projet

### Prérequis

- Docker
- Docker Compose

### Commande

Depuis le dossier `kongProject` :

```bash
docker-compose up -d

Services accessibles
Service	Port	URL
Kong Proxy	8000	http://localhost:8000
Kong Admin API	8001	http://localhost:8001
Konga UI	1337	http://localhost:1337
Metrics Prometheus	8001	http://localhost:8001/metrics
Tester une route protégée JWT

curl -i -X GET http://localhost:8000/api/products \
  -H "Authorization: Bearer <votre_token>"

Conclusion

Ce projet fournit une stack complète pour :

    Gérer vos APIs via Kong

    Appliquer une politique de sécurité complète

    Surveiller vos APIs avec Prometheus

    Documenter vos routes et configuration
