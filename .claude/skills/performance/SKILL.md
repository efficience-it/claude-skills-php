---
name: performance
description: "Audit de performance pour projet PHP/Symfony. Détecte les requêtes N+1, les problèmes d'hydratation Doctrine, les caches manquants, les requêtes lentes. Utiliser quand un endpoint est lent ou pour un audit de performance global."
---

# Audit de performance PHP/Symfony

## Périmètre

Analyser le fichier, le contrôleur ou l'endpoint indiqué. Si aucun périmètre n'est précisé, analyser les contrôleurs et repositories du projet.

## Critères d'analyse

### Doctrine - Requêtes (bloquant)

- **N+1** : boucle sur une collection avec accès à une relation lazy-loadée
  - Fix : ajouter un `JOIN` dans le QueryBuilder ou un `fetch: EAGER` ciblé
- **Flush dans une boucle** : `$em->flush()` appelé à chaque itération
  - Fix : un seul flush après la boucle
- **SELECT *** : requête qui charge toutes les colonnes quand seules quelques-unes sont nécessaires
  - Fix : utiliser un `SELECT partiel` ou un DTO
- **Requête sans index** : WHERE ou ORDER BY sur une colonne non indexée
  - Fix : ajouter un index via une migration

### Doctrine - Hydratation (important)

- Hydratation complète d'entités quand seuls quelques champs sont lus
  - Fix : `Query::HYDRATE_ARRAY` ou DTO projection
- Chargement de collections entières en mémoire
  - Fix : pagination, `iterate()`, ou `toIterable()`
- Entités inutilement trackées par l'UnitOfWork
  - Fix : `$em->detach()` ou `$em->clear()` pour le traitement par batch

### Cache (important)

- Résultat de requête coûteuse non caché (requête identique appelée plusieurs fois par request)
  - Fix : Symfony Cache ou Doctrine Result Cache
- Configuration récupérée depuis la DB à chaque requête
  - Fix : cache applicatif avec TTL adapté
- Données statiques recalculées à chaque appel
  - Fix : cache HTTP, cache Symfony, ou variable statique

### PHP (suggestion)

- Manipulation de tableaux non optimisée (`array_merge` dans une boucle)
  - Fix : spread operator ou `array_push`
- Chargement de fichiers volumineux en mémoire (`file_get_contents` sur un gros fichier)
  - Fix : stream, `SplFileObject`, ou `fopen`/`fgets`
- Regex compilée à chaque appel dans une boucle
  - Fix : sortir la regex de la boucle

### HTTP et réseau (important)

- Appels HTTP synchrones dans une requête web
  - Fix : Messenger pour le traitement asynchrone, ou HttpClient avec timeout
- Absence de timeout sur les appels externes
  - Fix : configurer `timeout` et `max_duration` sur HttpClient
- Appels HTTP séquentiels qui pourraient être parallèles
  - Fix : `HttpClient` avec réponses asynchrones

### Templates Twig (suggestion)

- Requêtes Doctrine dans les templates (via des méthodes d'entité qui triggent du lazy loading)
- Boucles avec filtre `|filter` ou `|sort` sur des collections non paginées
- Fragments non cachés (`{% cache %}`) pour les blocs statiques coûteux

## Format de sortie

Pour chaque problème :
- Fichier et ligne
- Type de problème
- Impact estimé (élevé / moyen / faible)
- Code problématique
- Code optimisé
- Gain attendu (qualitatif : "élimine N requêtes par item", "réduit la mémoire de O(n) à O(1)")
