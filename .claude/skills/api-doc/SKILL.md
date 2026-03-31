---
name: api-doc
description: "Génère la documentation OpenAPI pour les endpoints d'un contrôleur API Symfony. Produit les attributs OA (NelmioApiDocBundle) avec descriptions, paramètres, schémas et réponses."
---

# Documentation OpenAPI pour Symfony

## Processus

1. Lire le contrôleur API cible
2. Analyser chaque méthode/endpoint (route, paramètres, body, réponse)
3. Générer les attributs OpenAPI correspondants

## Ce qu'il faut documenter pour chaque endpoint

- `#[OA\Tag]` : groupement logique
- `#[OA\Parameter]` : chaque paramètre path et query
- `#[OA\RequestBody]` : schéma du body pour POST/PUT/PATCH
- `#[OA\Response]` : chaque code de réponse possible (200, 201, 400, 401, 403, 404, 422)

## Réponses à documenter systématiquement

- `200` : succès (GET, PUT, PATCH)
- `201` : création réussie (POST)
- `204` : suppression réussie (DELETE)
- `400` : requête invalide
- `401` : non authentifié
- `403` : non autorisé
- `404` : ressource non trouvée
- `422` : erreurs de validation

## Format de sortie

Générer les attributs OpenAPI directement au-dessus de chaque méthode du contrôleur. Le code doit être prêt à copier-coller.
