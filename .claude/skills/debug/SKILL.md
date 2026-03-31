---
name: debug
description: "Aide au débogage pour projet PHP/Symfony. Analyse une erreur, une stacktrace, ou un comportement inattendu. Identifie la cause probable et propose un fix. Utiliser quand un bug résiste ou qu'on ne comprend pas une erreur."
---

# Débogage PHP/Symfony

## Processus

1. Analyser l'erreur fournie (message, stacktrace, log, comportement décrit)
2. Identifier la cause probable
3. Vérifier l'hypothèse en lisant le code concerné
4. Proposer un fix

## Analyse par type d'erreur

### Exceptions Symfony

- Lire le message d'exception et la classe d'exception
- Remonter la stacktrace jusqu'au code applicatif (ignorer les frames internes Symfony)
- Identifier le fichier et la ligne dans le code du projet

### Erreurs Doctrine

- `MappingException` : vérifier les annotations/attributs de l'entité
- `QueryException` / `DQLException` : vérifier la syntaxe DQL et les noms de champs
- `UniqueConstraintViolationException` : identifier la contrainte et les données en doublon
- `TableNotFoundException` : migration manquante ou non exécutée
- Connection errors : vérifier `.env` et `DATABASE_URL`

### Erreurs HTTP

- 404 : vérifier le routing (`debug:router`), les paramètres de route, les requirements
- 403 : vérifier les voters, les firewalls, `security.yaml`
- 500 : lire les logs (`var/log/dev.log` ou `var/log/prod.log`)
- CORS : vérifier NelmioCorsBundle ou la config Apache/Nginx

### Erreurs de cache

- Symptôme : modification de code sans effet
- Vérifier : `bin/console cache:clear`, `var/cache/` supprimé, OPcache en dev
- Twig cache, Doctrine proxy cache, annotation cache

### Erreurs de serialization

- Circular reference : configurer les groupes de serialization
- Max depth : ajouter `#[MaxDepth]`
- Champs manquants : vérifier les groupes et les `#[Groups]`

### Erreurs Messenger

- Message non consommé : vérifier le routing et le transport
- Handler non appelé : vérifier `#[AsMessageHandler]` et l'autoconfigure
- Retry en boucle : vérifier les exceptions et le retry policy

## Outils de diagnostic

Suggérer les commandes Symfony pertinentes :

- `bin/console debug:router` : vérifier les routes
- `bin/console debug:container` : vérifier les services
- `bin/console debug:event-dispatcher` : vérifier les listeners
- `bin/console messenger:failed:show` : messages en échec
- `bin/console doctrine:schema:validate` : cohérence mapping/DB

## Format de sortie

1. **Diagnostic** : la cause identifiée en une phrase
2. **Explication** : pourquoi cette erreur se produit
3. **Fix** : le code corrigé
4. **Prévention** : comment éviter cette erreur à l'avenir (test, validation, config)
