---
name: command
description: "Génère une commande console Symfony. Produit une commande avec arguments, options, validation des inputs, progress bar, output structuré et gestion d'erreur."
---

# Génération de commande Symfony Console

## Structure

### Attribut PHP 8+
```php
#[AsCommand(name: 'app:entity:action', description: 'Description courte')]
```

### Nommage
Préfixe `app:`, format `app:domaine:action`.

## Règles

### Architecture
- La commande est un adaptateur mince : parse les inputs, appelle un service, affiche le résultat
- Pas de logique métier dans la commande
- Injecter les dépendances par constructeur

### Output
- Utiliser `SymfonyStyle`
- `$io->title()`, `$io->table()`, `$io->progressBar()`
- `$io->success()` / `$io->error()` pour le résultat final

### Dry-run
- Supporter `--dry-run` quand la commande modifie des données
- Afficher ce qui serait fait sans persister

### Traitement par batch
Flush et clear Doctrine tous les 100 items dans les boucles.

## Format de sortie

Classe de commande complète avec imports, attribut, configuration, et méthode execute.
