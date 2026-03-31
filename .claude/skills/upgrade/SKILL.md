---
name: upgrade
description: "Assiste la montée de version PHP ou Symfony. Analyse le projet pour détecter les deprecations, breaking changes, et incompatibilités. Produit un plan de migration."
---

# Montée de version PHP/Symfony

## Montée de version Symfony

### Analyse
- Lire `composer.json` pour la version actuelle
- Identifier tous les packages Symfony utilisés
- Vérifier la compatibilité des bundles tiers

### Détection des deprecations
- Annotations remplacées par des attributs PHP 8+
- Classes/méthodes marquées `@deprecated`
- Configurations YAML dépréciées
- `bin/console debug:container --deprecations`

### Plan de migration
1. Mettre à jour `composer.json`
2. Résoudre les conflits de dépendances
3. Corriger les deprecations
4. Mettre à jour les fichiers de configuration (recipes Flex)
5. Adapter le code aux breaking changes
6. Lancer les tests et corriger les régressions

## Montée de version PHP

### Points d'attention courants
- Enums (PHP 8.1+)
- Readonly properties (PHP 8.1+), readonly classes (PHP 8.2+)
- `#[\Override]` (PHP 8.3+)
- Deprecated dynamic properties (PHP 8.2+)
- Changements dans les fonctions natives

## Format de sortie

Résumé, breaking changes, deprecations, dépendances tierces à mettre à jour, plan d'exécution ordonné.
