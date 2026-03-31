---
name: docker
description: "Génère un environnement Docker pour projet PHP/Symfony. Produit un docker-compose.yml avec PHP-FPM, Nginx/Caddy, base de données, et services annexes."
---

# Environnement Docker pour PHP/Symfony

## Détection des besoins

Depuis `composer.json`, `.env`, et la configuration :
- Version PHP, extensions nécessaires
- Base de données : `DATABASE_URL`
- Redis : `REDIS_URL`
- RabbitMQ : `MESSENGER_TRANSPORT_DSN` avec `amqp://`
- Mailer : `MAILER_DSN`
- Node.js : `package.json` présent

## Services

- **PHP-FPM** : version alignée sur composer.json, Xdebug configurable, OPcache
- **Caddy** (recommandé) ou **Nginx**
- **PostgreSQL/MySQL** selon DATABASE_URL, volume persistant
- **Redis**, **RabbitMQ**, **Mailpit** selon les besoins
- **Node** si package.json présent

## Bonnes pratiques

- `.dockerignore` : exclure vendor/, node_modules/, var/, .git/
- Healthcheck sur chaque service
- Variables d'environnement dans .env
- Makefile avec les commandes courantes

## Format de sortie

Générer tous les fichiers nécessaires (docker-compose.yml, Dockerfile, configs), prêts à `docker compose up`.
