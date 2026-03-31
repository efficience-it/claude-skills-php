---
name: phpstan
description: "Corrige les erreurs PHPStan dans un projet PHP/Symfony. Lance l'analyse, interprète les erreurs, et applique les corrections sans jamais utiliser @phpstan-ignore ni réduire le niveau. Utiliser quand PHPStan remonte des erreurs à corriger."
---

# Correction des erreurs PHPStan

## Processus

1. Lancer PHPStan : `vendor/bin/phpstan analyse` (ou la commande configurée dans le projet)
2. Parser la sortie pour identifier chaque erreur
3. Corriger par ordre de priorité
4. Relancer pour vérifier que les corrections n'introduisent pas de nouvelles erreurs

## Règles absolues

- Ne JAMAIS ajouter `@phpstan-ignore-*` pour masquer une erreur
- Ne JAMAIS réduire le niveau PHPStan dans la configuration
- Ne JAMAIS ajouter d'entrée dans `ignoreErrors` du fichier `phpstan.neon`
- Chaque correction doit résoudre le problème réel, pas le symptôme

## Corrections par type d'erreur

### Appel de méthode sur un type nullable

```php
// Avant (erreur)
$user->getName();  // $user peut être null

// Après
if ($user === null) {
    throw new UserNotFoundException();
}
$user->getName();
```

Ne pas utiliser l'opérateur `?->` comme solution par défaut. Se demander : est-ce que null est un cas légitime ici ?

### Type de retour incorrect

- Ajouter le type de retour correct
- Si la méthode retourne plusieurs types, utiliser un type union (`string|null`)
- Si c'est un problème de PHPDoc incorrect, corriger le PHPDoc

### Paramètre manquant ou type incorrect

- Ajouter le type hint correct
- Vérifier les appelants pour s'assurer de la compatibilité

### Propriété non initialisée

- Ajouter une valeur par défaut si logique
- Marquer comme nullable si c'est le cas
- Initialiser dans le constructeur

### Classe/méthode inexistante

- Vérifier l'import (use statement)
- Vérifier que le package est installé
- Corriger le nom si c'est une typo

### Dead code

- Supprimer le code mort si confirmé
- Vérifier qu'il n'y a pas de magie (reflection, container) avant de supprimer

### Generics et templates

- Ajouter les annotations `@template` et `@extends` correctes
- Typer les collections (`Collection<int, Entity>` au lieu de `Collection`)

## Format de sortie

Pour chaque erreur corrigée :
- L'erreur PHPStan originale
- Le fichier et la ligne
- La correction appliquée
- Explication courte si la correction n'est pas évidente
