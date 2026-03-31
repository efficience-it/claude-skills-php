# Claude Skills PHP & Symfony

Skills Claude Code pour les développeurs PHP et Symfony. Des prompts réutilisables, prêts à l'emploi, que toute l'équipe invoque avec un simple `/nom-du-skill`.

## Installation

Copiez les skills dans votre projet :

```bash
# Un skill spécifique
cp -r skills/review/.claude/skills/review .claude/skills/

# Tous les skills
cp -r skills/*/.claude/skills/* .claude/skills/
```

Commitez le dossier `.claude/skills/` dans votre repo. Toute l'équipe y accède immédiatement.

## Skills disponibles

### Qualité de code

| Skill | Description |
|-------|-------------|
| [/review](skills/review) | Revue de code standardisée : architecture, sécurité, tests, conventions |
| [/refactor](skills/refactor) | Refactoring guidé par l'architecture hexagonale et les principes SOLID |
| [/phpstan](skills/phpstan) | Corrige les erreurs PHPStan sans jamais utiliser `@phpstan-ignore` |
| [/deprecations](skills/deprecations) | Scanne les deprecations PHP et Symfony, propose les remplacements |

### Tests & données

| Skill | Description |
|-------|-------------|
| [/test](skills/test) | Génère des tests PHPUnit : cas nominal, limites, erreurs, data providers |
| [/fixtures](skills/fixtures) | Génère des fixtures réalistes (Alice/DoctrineFixturesBundle) |

### Sécurité & performance

| Skill | Description |
|-------|-------------|
| [/security](skills/security) | Audit de sécurité basé sur l'OWASP Top 10 |
| [/performance](skills/performance) | Détecte les N+1, caches manquants, requêtes lentes |

### Génération de code

| Skill | Description |
|-------|-------------|
| [/crud](skills/crud) | CRUD complet : entité, repository, contrôleur, form, templates, tests |
| [/dto](skills/dto) | DTOs readonly avec validation et mapping entité |
| [/command](skills/command) | Commande Symfony Console avec arguments, progress bar, dry-run |
| [/api-doc](skills/api-doc) | Documentation OpenAPI (NelmioApiDocBundle) |
| [/api-platform](skills/api-platform) | Ressource API Platform : opérations, filtres, sérialisation |

### Infrastructure & CI

| Skill | Description |
|-------|-------------|
| [/ci](skills/ci) | Pipeline CI/CD (GitHub Actions / GitLab CI) |
| [/docker](skills/docker) | Environnement Docker complet (PHP-FPM, Caddy, DB, Redis, Mailpit) |

### Symfony

| Skill | Description |
|-------|-------------|
| [/migration](skills/migration) | Vérifie les migrations Doctrine : perte de données, zero-downtime |
| [/messenger](skills/messenger) | Audit de la configuration Messenger : routage, retry, handlers |

### Planification & onboarding

| Skill | Description |
|-------|-------------|
| [/plan](skills/plan) | Planifie l'implémentation d'une feature avant de coder |
| [/debug](skills/debug) | Analyse une erreur ou stacktrace et propose un fix |
| [/upgrade](skills/upgrade) | Assiste la montée de version PHP ou Symfony |
| [/onboard](skills/onboard) | Génère un guide d'onboarding technique pour les nouveaux arrivants |

## Utilisation

Dans Claude Code, tapez `/` suivi du nom du skill :

```
/review          # Revue du code staged
/test            # Génère des tests pour le fichier courant
/plan            # Planifie une feature
/security        # Audit de sécurité
```

## Personnalisation

Chaque skill est un fichier Markdown dans `.claude/skills/nom-du-skill/SKILL.md`. Adaptez-les à vos conventions :

- Ajoutez vos règles d'architecture
- Précisez vos conventions de nommage
- Ajoutez les patterns spécifiques à votre projet

## Contribuer

Les PRs sont les bienvenues. Un skill = un dossier dans `skills/` avec un `SKILL.md`.

## Licence

MIT

---

Maintenu par [Efficience IT](https://www.itefficience.com) - Expertise PHP & Symfony
