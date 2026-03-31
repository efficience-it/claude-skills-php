---
name: docker
description: "Génère un environnement Docker pour projet PHP/Symfony. Produit un docker-compose.yml avec PHP-FPM, Nginx/Caddy, base de données, et services annexes. Utiliser pour mettre en place ou améliorer l'environnement de dev Docker."
---

# Environnement Docker pour PHP/Symfony

## Processus

1. Analyser le projet (PHP version, DB, services requis dans .env)
2. Détecter un Docker existant (docker-compose.yml, Dockerfile)
3. Générer ou améliorer la configuration

## Détection des besoins

Depuis `composer.json`, `.env`, et la configuration :

- Version PHP requise
- Extensions PHP nécessaires (intl, pdo_pgsql, gd, etc.)
- Base de données : `DATABASE_URL` dans `.env`
- Redis : `REDIS_URL` ou bundle `snc/redis-bundle`
- RabbitMQ : `MESSENGER_TRANSPORT_DSN` avec `amqp://`
- Elasticsearch : bundle FOS/Elastica ou configuration
- Mailer : `MAILER_DSN` dans `.env`
- Node.js : `package.json` présent

## Services

### PHP-FPM

- Version alignée sur `composer.json`
- Extensions détectées automatiquement
- Xdebug configuré mais désactivé par défaut
- Composer installé dans l'image
- OPcache configuré pour le dev (validate_timestamps=1)

### Serveur web

- **Caddy** (recommandé) : config simple, HTTPS automatique en local
- **Nginx** : si le projet a déjà une config Nginx

### Base de données

- PostgreSQL ou MySQL selon `DATABASE_URL`
- Volume persistant pour les données
- Script d'initialisation si nécessaire
- Port exposé pour les outils GUI (DBeaver, TablePlus)

### Services optionnels

- **Redis** : cache, sessions, ou Messenger transport
- **RabbitMQ** : Messenger avec management UI
- **Mailpit** : capture des emails en dev (remplace Mailhog)
- **Elasticsearch/Meilisearch** : si détecté dans le projet
- **Node** : si package.json présent (build assets)

## Structure des fichiers

```
docker/
├── php/
│   ├── Dockerfile
│   └── conf.d/
│       └── xdebug.ini
├── caddy/
│   └── Caddyfile
└── nginx/
    └── default.conf
docker-compose.yml
.dockerignore
```

## Bonnes pratiques

- `.dockerignore` : exclure `vendor/`, `node_modules/`, `var/`, `.git/`
- Volumes pour `vendor/` et `var/` : performances sur macOS/Windows
- Healthcheck sur chaque service
- Variables d'environnement dans `.env` (pas en dur dans docker-compose)
- Makefile avec les commandes courantes (up, down, console, composer, test)

## Format de sortie

Générer tous les fichiers nécessaires (docker-compose.yml, Dockerfile, configs), prêts à `docker compose up`.
