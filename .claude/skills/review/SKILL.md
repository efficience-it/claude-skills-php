---
name: review
description: "Revue de code standardisée pour projet PHP/Symfony. Analyse le diff git staged ou un fichier donné. Détecte les violations d'architecture, failles de sécurité, tests manquants, et violations PSR-12."
---

# Revue de code PHP/Symfony

## Contexte

Analyse le diff git staged (`git diff --cached`). Si aucun fichier n'est staged, analyse le diff non staged (`git diff`). Si un fichier ou dossier est spécifié par l'utilisateur, analyse uniquement celui-ci.

## Critères d'analyse

### Architecture (bloquant)

- Logique métier dans un contrôleur, une commande console, ou un listener
- Dépendance du Domain vers l'Infrastructure ou vers le Framework
- Import Symfony dans une classe du Domain (Entity, ValueObject, Service métier)
- Repository qui contient de la logique métier au lieu de requêtes
- Service applicatif qui accède directement à `$_GET`, `$_POST`, `$_SESSION` ou `Request`

### Sécurité (bloquant)

- Requêtes DQL/SQL construites par concaténation au lieu de paramètres bindés
- Données utilisateur affichées dans Twig sans échappement (`|raw` sur des données non fiables)
- Contrôleur ou route sans contrôle d'accès (`#[IsGranted]`, `denyAccessUnlessGranted`, voter)
- Tokens, mots de passe, clés API en dur dans le code
- Désérialisation de données utilisateur sans validation
- Upload de fichier sans vérification du type MIME réel

### Tests (important)

- Classe métier sans test unitaire correspondant
- Cas limites non couverts (null, chaîne vide, collection vide, valeurs extrêmes)
- Test fonctionnel qui mocke la base de données au lieu de la tester réellement
- Absence de data provider quand plusieurs cas similaires sont testés

### Conventions (suggestion)

- Violations PSR-12 (nommage, espacement, structure)
- Nommage incohérent avec le reste du projet
- Méthode de plus de 30 lignes sans extraction
- Classe de plus de 300 lignes
- Injection par constructeur manquante (utilisation de `new` pour un service)

### Doctrine (important)

- Relation sans cascade appropriée
- Requête N+1 détectable (boucle sur une collection lazy-loadée)
- Absence d'index sur une colonne utilisée dans un WHERE ou ORDER BY
- Flush dans une boucle au lieu d'un flush unique

## Format de sortie

Classe chaque remarque par sévérité : bloquant, important, suggestion. Pour chaque remarque : fichier et ligne, description du problème, exemple de correction.
