---
name: debug
description: "Aide au débogage pour projet PHP/Symfony. Analyse une erreur, une stacktrace, ou un comportement inattendu. Identifie la cause probable et propose un fix."
---

# Débogage PHP/Symfony

## Analyse par type d'erreur

### Exceptions Symfony
Remonter la stacktrace jusqu'au code applicatif (ignorer les frames internes Symfony).

### Erreurs Doctrine
- `MappingException` : vérifier les annotations/attributs de l'entité
- `QueryException` : vérifier la syntaxe DQL et les noms de champs
- `UniqueConstraintViolationException` : identifier la contrainte et les données en doublon
- `TableNotFoundException` : migration manquante ou non exécutée

### Erreurs HTTP
- 404 : vérifier le routing (`debug:router`)
- 403 : vérifier les voters, firewalls, `security.yaml`
- 500 : lire les logs (`var/log/dev.log` ou `var/log/prod.log`)

### Erreurs de cache
Symptôme : modification de code sans effet. Vérifier : `bin/console cache:clear`, `var/cache/`, OPcache.

### Erreurs Messenger
- Message non consommé : vérifier le routing et le transport
- Handler non appelé : vérifier `#[AsMessageHandler]` et l'autoconfigure

## Outils de diagnostic

- `bin/console debug:router`
- `bin/console debug:container`
- `bin/console debug:event-dispatcher`
- `bin/console messenger:failed:show`
- `bin/console doctrine:schema:validate`

## Format de sortie

1. **Diagnostic** : la cause en une phrase
2. **Explication** : pourquoi cette erreur se produit
3. **Fix** : le code corrigé
4. **Prévention** : comment éviter cette erreur à l'avenir
