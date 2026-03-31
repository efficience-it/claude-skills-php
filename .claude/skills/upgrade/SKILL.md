---
name: upgrade
description: "Assiste la montée de version PHP ou Symfony. Analyse le projet pour détecter les deprecations, breaking changes, et incompatibilités. Produit un plan de migration étape par étape. Utiliser avant ou pendant une montée de version."
---

# Montée de version PHP/Symfony

## Processus

1. Identifier la version actuelle (composer.json, composer.lock)
2. Identifier la version cible (demandée par l'utilisateur ou dernière stable)
3. Analyser les breaking changes entre les deux versions
4. Scanner le code pour les deprecations et incompatibilités
5. Produire un plan de migration

## Montée de version Symfony

### Analyse

- Lire `composer.json` pour la version actuelle de `symfony/*`
- Identifier tous les packages Symfony utilisés
- Vérifier la compatibilité des bundles tiers avec la version cible

### Détection des deprecations

Scanner le code pour :
- Annotations remplacées par des attributs PHP 8+
- Classes/méthodes marquées `@deprecated` dans la version actuelle
- Configurations YAML dépréciées
- Patterns remplacés (ex: `TreeBuilder` sans `getRootNode()`, services publics par défaut)

### Plan de migration

1. Mettre à jour `composer.json` avec les nouvelles contraintes
2. Résoudre les conflits de dépendances
3. Corriger les deprecations identifiées
4. Mettre à jour les fichiers de configuration (recipes Flex)
5. Adapter le code aux breaking changes
6. Lancer les tests et corriger les régressions

### Ressources

- UPGRADE-X.Y.md dans le repo Symfony
- CHANGELOG-X.Y.md pour les détails
- `bin/console debug:container --deprecations` pour les deprecations runtime

## Montée de version PHP

### Analyse

- Version actuelle dans `composer.json` (`php` constraint)
- Features utilisées qui changent de comportement
- Extensions PHP requises

### Points d'attention courants

- Changements de typage strict (PHP 8.0+)
- Enums (PHP 8.1+)
- Readonly properties (PHP 8.1+), readonly classes (PHP 8.2+)
- Intersection types (PHP 8.1+), DNF types (PHP 8.2+)
- `#[\Override]` (PHP 8.3+)
- Deprecated dynamic properties (PHP 8.2+)
- Changements dans les fonctions natives (paramètres, valeurs de retour)

## Format de sortie

### 1. Résumé

- Version actuelle et version cible
- Nombre de deprecations trouvées
- Estimation de complexité

### 2. Breaking changes

Liste des changements qui cassent le code, avec le fichier et la correction.

### 3. Deprecations

Liste des deprecations avec le remplacement recommandé.

### 4. Dépendances tierces

Bundles et packages à mettre à jour, avec la version compatible.

### 5. Plan d'exécution

Étapes ordonnées, chacune mergeable indépendamment si possible.
