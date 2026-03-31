---
name: ci
description: "Génère ou audite un pipeline CI/CD pour projet PHP/Symfony. Supporte GitHub Actions et GitLab CI. Couvre tests, PHPStan, CS-Fixer, sécurité, et déploiement. Utiliser pour mettre en place ou améliorer la CI d'un projet."
---

# CI/CD pour projet PHP/Symfony

## Processus

1. Détecter la plateforme CI existante (.github/workflows/, .gitlab-ci.yml) ou demander
2. Analyser le projet (PHP version, DB, services requis, outils qualité)
3. Générer ou auditer le pipeline

## Détection de l'existant

- `.github/workflows/*.yml` : GitHub Actions
- `.gitlab-ci.yml` : GitLab CI
- `Makefile`, `composer.json` scripts : commandes disponibles
- `phpstan.neon`, `.php-cs-fixer.php`, `phpunit.xml` : outils configurés

## Étapes du pipeline

### 1. Setup PHP

- Version PHP du projet (depuis `composer.json`)
- Extensions requises (intl, pdo_pgsql, etc.)
- Composer install avec cache

### 2. Qualité de code

- **PHP-CS-Fixer** : `vendor/bin/php-cs-fixer fix --dry-run --diff`
- **PHPStan** : `vendor/bin/phpstan analyse`
- **Lint Twig** : `bin/console lint:twig templates/`
- **Lint YAML** : `bin/console lint:yaml config/`
- **Lint Container** : `bin/console lint:container`

### 3. Tests

- **PHPUnit** : `vendor/bin/phpunit`
- Services requis (MySQL/PostgreSQL, Redis, RabbitMQ) en tant que services CI
- Base de données de test : création, migrations, fixtures
- Coverage report si configuré

### 4. Sécurité

- `composer audit` : vérifier les CVE dans les dépendances
- `bin/console security:check` si disponible

### 5. Assets (si applicable)

- `npm ci` et `npm run build`
- Vérification que les assets compilés sont cohérents

## GitHub Actions - Template

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: shivammathur/setup-php@v2
        with:
          php-version: '8.3'
          extensions: intl, pdo_pgsql
          coverage: none
      - run: composer install --no-progress
      - run: vendor/bin/php-cs-fixer fix --dry-run --diff
      - run: vendor/bin/phpstan analyse

  tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test
        ports:
          - 5432:5432
    steps:
      - uses: actions/checkout@v4
      - uses: shivammathur/setup-php@v2
        with:
          php-version: '8.3'
          extensions: intl, pdo_pgsql
      - run: composer install --no-progress
      - run: bin/console doctrine:migrations:migrate --no-interaction
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/test
      - run: vendor/bin/phpunit
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/test
```

## Audit d'un pipeline existant

Vérifier :
- Cache Composer configuré (accélère de 30-60s)
- Jobs parallèles quand possible (quality + tests en parallèle)
- Fail fast : qualité de code avant les tests (plus rapide à échouer)
- Services CI correctement configurés (healthcheck, ports)
- Variables d'environnement pour la DB de test
- Version PHP identique à la production

## Format de sortie

Générer le fichier de workflow complet, prêt à commiter. Adapter aux outils et services détectés dans le projet.
