---
name: plan
description: "Planifie l'implémentation d'une feature ou d'un ticket dans un projet PHP/Symfony. Analyse le contexte, identifie les fichiers à créer/modifier, propose un ordre d'exécution et liste les risques. Utiliser avant de coder pour cadrer le travail."
---

# Planification d'implémentation

## Processus

1. Comprendre le besoin (ticket, description, conversation)
2. Analyser le code existant qui sera impacté
3. Identifier l'architecture en place et les patterns du projet
4. Proposer un plan structuré

## Structure du plan

### 1. Résumé

Une phrase qui reformule le besoin pour validation.

### 2. Analyse de l'existant

- Fichiers et classes qui seront impactés
- Patterns déjà utilisés dans le projet pour des cas similaires
- Contraintes identifiées (dette technique, couplage, dépendances)

### 3. Plan d'implémentation

Pour chaque étape, indiquer :
- Le fichier à créer ou modifier
- Ce qui change et pourquoi
- Les dépendances avec les autres étapes

Ordonner les étapes pour que le code compile et les tests passent à chaque étape intermédiaire.

### 4. Fichiers à créer

- Chemin complet
- Responsabilité de la classe
- Namespace et couche architecturale

### 5. Fichiers à modifier

- Chemin complet
- Nature de la modification
- Risque d'impact sur d'autres fonctionnalités

### 6. Tests à écrire

- Tests unitaires nécessaires
- Tests fonctionnels nécessaires
- Cas limites à couvrir

### 7. Risques et points d'attention

- Impact sur les fonctionnalités existantes
- Migrations de base de données nécessaires
- Changements de configuration
- Compatibilité ascendante à préserver

### 8. Estimation de complexité

- Nombre de fichiers impactés
- Complexité relative (simple / modéré / complexe)
- Pré-requis éventuels (autre ticket, autre PR, migration)

## Règles

- Ne pas proposer d'over-engineering : la solution la plus simple qui répond au besoin
- Respecter les patterns existants du projet, même s'ils ne sont pas idéaux
- Signaler les opportunités de refactoring mais ne pas les inclure dans le plan sauf demande explicite
- Chaque étape doit être mergeable indépendamment si possible
