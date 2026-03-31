---
name: onboard
description: "Génère un guide d'onboarding technique pour un projet PHP/Symfony. Analyse le code source, la stack, l'architecture, les commandes et les conventions."
---

# Onboarding technique

## Sections du guide

### 1. Stack technique

Version PHP, Symfony, base de données, outils (Redis, RabbitMQ, etc.), bundles principaux.

### 2. Installation et démarrage

Commandes d'installation, variables d'environnement, base de données (création, migrations, fixtures), serveur de dev, Docker si présent.

### 3. Architecture

Structure des dossiers `src/`, pattern architectural (MVC, hexagonal, DDD, CQRS), couches et responsabilités, conventions de nommage.

### 4. Commandes utiles

Tests, linter, PHPStan, CS-Fixer, migrations, cache, assets, Makefile.

### 5. Conventions de code

Standard de codage (PSR-12, custom), organisation des tests, git workflow, CI/CD.

### 6. Points d'attention

Zones de dette technique, fichiers sensibles, particularités du projet, dépendances obsolètes.

## Format de sortie

Document structuré en Markdown, factuel et concis. Pas de suppositions : uniquement ce qui est vérifiable dans le code source.
