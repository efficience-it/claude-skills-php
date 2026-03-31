---
name: performance
description: "Audit de performance pour projet PHP/Symfony. Détecte les requêtes N+1, les problèmes d'hydratation Doctrine, les caches manquants, les requêtes lentes."
---

# Audit de performance PHP/Symfony

## Critères d'analyse

### Doctrine - Requêtes (bloquant)

- **N+1** : boucle sur une collection avec accès à une relation lazy-loadée
- **Flush dans une boucle** : `$em->flush()` appelé à chaque itération
- **SELECT *** : requête qui charge toutes les colonnes quand seules quelques-unes sont nécessaires
- **Requête sans index** : WHERE ou ORDER BY sur une colonne non indexée

### Doctrine - Hydratation (important)

- Hydratation complète d'entités quand seuls quelques champs sont lus
- Chargement de collections entières en mémoire
- Entités inutilement trackées par l'UnitOfWork

### Cache (important)

- Résultat de requête coûteuse non caché
- Configuration récupérée depuis la DB à chaque requête
- Données statiques recalculées à chaque appel

### HTTP et réseau (important)

- Appels HTTP synchrones dans une requête web
- Absence de timeout sur les appels externes
- Appels HTTP séquentiels qui pourraient être parallèles

### Templates Twig (suggestion)

- Requêtes Doctrine dans les templates (lazy loading)
- Boucles avec filtre coûteux sur de grandes collections

## Format de sortie

Pour chaque problème : fichier et ligne, type, impact estimé, code problématique, code optimisé, gain attendu.
