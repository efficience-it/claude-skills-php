---
name: plan
description: "Planifie l'implémentation d'une feature ou d'un ticket dans un projet PHP/Symfony. Analyse le contexte, identifie les fichiers à créer/modifier, propose un ordre d'exécution et liste les risques."
---

# Planification d'implémentation

## Structure du plan

### 1. Résumé
Une phrase qui reformule le besoin pour validation.

### 2. Analyse de l'existant
Fichiers et classes impactés, patterns déjà utilisés, contraintes identifiées.

### 3. Plan d'implémentation
Pour chaque étape : fichier à créer ou modifier, ce qui change et pourquoi, dépendances avec les autres étapes. Ordonner pour que le code compile à chaque étape intermédiaire.

### 4. Fichiers à créer
Chemin complet, responsabilité, namespace et couche architecturale.

### 5. Fichiers à modifier
Chemin complet, nature de la modification, risque d'impact.

### 6. Tests à écrire
Tests unitaires, fonctionnels, cas limites à couvrir.

### 7. Risques et points d'attention
Impact sur l'existant, migrations nécessaires, changements de configuration, compatibilité ascendante.

### 8. Estimation de complexité
Nombre de fichiers impactés, complexité relative (simple / modéré / complexe), pré-requis éventuels.

## Règles

- Ne pas proposer d'over-engineering
- Respecter les patterns existants du projet
- Signaler les opportunités de refactoring sans les inclure sauf demande explicite
