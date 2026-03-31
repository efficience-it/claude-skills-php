---
name: refactor
description: "Refactoring guidé par l'architecture pour projet PHP/Symfony. Respecte la séparation Domain/Application/Infrastructure. Explique chaque déplacement de code. Utiliser quand on veut restructurer du code en respectant l'architecture hexagonale ou les bonnes pratiques SOLID."
---

# Refactoring PHP/Symfony

## Processus

1. Lire le fichier ou la classe cible
2. Identifier l'architecture du projet (hexagonale, MVC classique, DDD)
3. Détecter les violations et les opportunités de refactoring
4. Proposer et appliquer les modifications en expliquant chaque décision

## Détection de l'architecture du projet

Inspecter la structure `src/` pour identifier le pattern :

- `src/Domain/`, `src/Application/`, `src/Infrastructure/` : architecture hexagonale
- `src/Entity/`, `src/Controller/`, `src/Repository/` : MVC Symfony classique
- `src/BoundedContext/` ou namespaces métier : DDD
- Hybrid : mélange des approches

Adapter les recommandations à l'architecture en place.

## Règles de refactoring

### Architecture hexagonale

- Le Domain ne dépend de rien : pas d'import Symfony, Doctrine, ou Infrastructure
- Les use cases vivent dans Application/ et orchestrent les appels
- Les contrôleurs sont des adaptateurs minces : pas de logique, juste de la délégation
- Les repositories sont dans Infrastructure/ et implémentent une interface du Domain
- Les events du Domain sont des objets simples, pas des events Symfony

### Principes SOLID

- **S** : une classe = une responsabilité. Si la description contient "et", extraire.
- **O** : préférer l'extension (interfaces, stratégies) à la modification de code existant
- **L** : les sous-types doivent être substituables sans casser le comportement
- **I** : des interfaces petites et spécifiques plutôt qu'une interface monolithique
- **D** : dépendre des abstractions (interfaces) pas des implémentations concrètes

### Extractions courantes

- Méthode > 20 lignes : extraire en méthodes privées nommées par intention
- Classe > 250 lignes : identifier les responsabilités et extraire
- Contrôleur avec logique métier : extraire vers un service applicatif ou un use case
- Repository avec logique métier : extraire vers un service du Domain
- Listener/Subscriber avec logique complexe : extraire vers un service dédié
- Conditions imbriquées : extraire en early returns ou en stratégies

### Ce qu'il ne faut PAS faire

- Ne pas créer d'abstraction pour un seul cas d'utilisation
- Ne pas extraire une interface si une seule classe l'implémente (sauf pour le Domain)
- Ne pas renommer sans raison : le renommage a un coût (références, habitudes)
- Ne pas refactorer et changer le comportement en même temps

## Format de sortie

Pour chaque modification :
1. Ce qui change et pourquoi
2. Le code avant
3. Le code après
4. Les fichiers impactés (imports, références, config)

Proposer un ordre d'exécution des changements pour que le code compile à chaque étape.
