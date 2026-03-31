---
name: phpstan
description: "Corrige les erreurs PHPStan dans un projet PHP/Symfony. Lance l'analyse, interprète les erreurs, et applique les corrections sans jamais utiliser @phpstan-ignore ni réduire le niveau."
---

# Correction des erreurs PHPStan

## Règles absolues

- Ne JAMAIS ajouter `@phpstan-ignore-*` pour masquer une erreur
- Ne JAMAIS réduire le niveau PHPStan dans la configuration
- Ne JAMAIS ajouter d'entrée dans `ignoreErrors` du fichier `phpstan.neon`
- Chaque correction doit résoudre le problème réel, pas le symptôme

## Corrections par type d'erreur

### Appel de méthode sur un type nullable

Ne pas utiliser `?->` comme solution par défaut. Se demander : est-ce que null est un cas légitime ?

### Type de retour incorrect

Ajouter le type de retour correct. Si la méthode retourne plusieurs types, utiliser un type union.

### Propriété non initialisée

Ajouter une valeur par défaut, marquer comme nullable, ou initialiser dans le constructeur.

### Dead code

Supprimer le code mort si confirmé. Vérifier qu'il n'y a pas de magie (reflection, container) avant de supprimer.

### Generics et templates

Ajouter les annotations `@template` et `@extends`. Typer les collections (`Collection<int, Entity>`).

## Format de sortie

Pour chaque erreur corrigée : l'erreur PHPStan originale, le fichier et la ligne, la correction appliquée, explication courte si non évidente.
