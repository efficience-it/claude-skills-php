---
name: migration
description: "Analyse et sécurise les migrations Doctrine. Vérifie la perte de données, la cohérence up/down, les requêtes longues, et la compatibilité zero-downtime."
---

# Vérification des migrations Doctrine

## Processus

1. Trouver la ou les migrations récentes dans `migrations/`
2. Analyser chaque opération SQL
3. Vérifier les critères ci-dessous
4. Proposer les corrections

## Critères d'analyse

### Perte de données (bloquant)

- `DROP TABLE` sans vérification que la table est vide ou que les données ont été migrées
- `DROP COLUMN` sans migration de données préalable
- `TRUNCATE` dans une migration
- Modification de type de colonne qui tronque des données (VARCHAR(255) vers VARCHAR(50))
- Suppression d'une contrainte UNIQUE qui pourrait créer des doublons

### Cohérence up/down (bloquant)

- Méthode `down()` absente
- `down()` qui ne fait pas l'inverse exact de `up()`
- `down()` qui tente de recréer une colonne supprimée sans restaurer les données

### Performance (important)

- `ALTER TABLE` sur une table volumineuse (> 1M lignes) sans estimation de temps
- Ajout d'index sur une grosse table sans `CONCURRENTLY` (PostgreSQL) ou équivalent
- Plusieurs `ALTER TABLE` sur la même table au lieu d'un seul
- `UPDATE` massif sans clause `WHERE` limitante

### Zero-downtime (important)

- Renommage de colonne (casse l'ancien code encore en prod)
- Suppression de colonne encore référencée dans le code
- Ajout de colonne NOT NULL sans valeur par défaut
- Modification de type de colonne

## Recommandation zero-downtime

Pour les opérations destructives, recommander le pattern en 3 étapes :
1. Migration 1 : ajouter la nouvelle colonne/table
2. Déploiement du code qui écrit dans les deux
3. Migration 2 : migrer les données, supprimer l'ancien
