---
name: ci
description: "Génère ou audite un pipeline CI/CD pour projet PHP/Symfony. Supporte GitHub Actions et GitLab CI. Couvre tests, PHPStan, CS-Fixer, sécurité."
---

# CI/CD pour projet PHP/Symfony

## Étapes du pipeline

### 1. Setup PHP
Version PHP du projet, extensions requises, Composer install avec cache.

### 2. Qualité de code
- PHP-CS-Fixer : `vendor/bin/php-cs-fixer fix --dry-run --diff`
- PHPStan : `vendor/bin/phpstan analyse`
- Lint Twig/YAML/Container

### 3. Tests
- PHPUnit avec services CI (DB, Redis, RabbitMQ)
- Base de données de test : création, migrations, fixtures

### 4. Sécurité
- `composer audit` : vérifier les CVE

## Audit d'un pipeline existant

Vérifier : cache Composer, jobs parallèles, fail fast, services CI correctement configurés, version PHP identique à la production.

## Format de sortie

Fichier de workflow complet, prêt à commiter, adapté aux outils et services du projet.
