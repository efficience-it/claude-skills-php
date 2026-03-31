---
name: onboard
description: "Génère un guide d'onboarding technique pour un projet PHP/Symfony. Analyse le code source, la stack, l'architecture, les commandes et les conventions. Utiliser quand un nouveau développeur rejoint le projet ou quand on veut un état des lieux."
---

# Onboarding technique

## Processus

1. Analyser les fichiers de configuration (composer.json, .env, docker-compose, Makefile)
2. Inspecter la structure `src/` et `tests/`
3. Lire le CLAUDE.md et le README s'ils existent
4. Identifier les patterns et conventions du projet
5. Générer le guide

## Sections du guide

### 1. Stack technique

- Version PHP
- Version Symfony
- Base de données (MySQL, PostgreSQL, SQLite)
- Outils : Redis, RabbitMQ, Elasticsearch, etc.
- Bundles principaux installés (API Platform, EasyAdmin, etc.)

### 2. Installation et démarrage

- Commandes d'installation (`composer install`, `npm install`, etc.)
- Variables d'environnement à configurer
- Base de données : création, migrations, fixtures
- Commande pour lancer le serveur de dev
- Docker si présent

### 3. Architecture

- Structure des dossiers `src/`
- Pattern architectural (MVC, hexagonal, DDD, CQRS)
- Couches et leurs responsabilités
- Conventions de nommage des classes

### 4. Commandes utiles

- Lancer les tests
- Lancer le linter / PHPStan / CS-Fixer
- Générer une migration
- Vider le cache
- Compiler les assets
- Makefile ou scripts npm si présents

### 5. Conventions de code

- Standard de codage (PSR-12, custom)
- Organisation des tests (Unit / Functional / Integration)
- Git workflow (branches, commits, PR)
- CI/CD pipeline

### 6. Points d'attention

- Zones de dette technique identifiables
- Fichiers ou dossiers sensibles
- Particularités du projet (legacy, multi-tenant, etc.)
- Dépendances obsolètes ou à surveiller

## Format de sortie

Un document structuré en Markdown, factuel et concis. Pas de suppositions : uniquement ce qui est vérifiable dans le code source. Indiquer clairement quand une information manque ("Non documenté", "À vérifier avec l'équipe").
