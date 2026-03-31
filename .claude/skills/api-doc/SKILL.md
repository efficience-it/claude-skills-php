---
name: api-doc
description: "Génère la documentation OpenAPI pour les endpoints d'un contrôleur API Symfony. Produit les attributs OA\\ (NelmioApiDocBundle) avec descriptions, paramètres, schémas de requête et réponses. Utiliser quand on veut documenter une API REST."
---

# Documentation OpenAPI pour Symfony

## Processus

1. Lire le contrôleur API cible
2. Analyser chaque méthode/endpoint (route, paramètres, body, réponse)
3. Générer les attributs OpenAPI correspondants

## Conventions

### Attributs OpenAPI (NelmioApiDocBundle)

Utiliser les attributs PHP 8+ `#[OA\...]` de `OpenApi\Attributes` :

```php
use OpenApi\Attributes as OA;
use Nelmio\ApiDocBundle\Attribute\Model;
```

### Ce qu'il faut documenter pour chaque endpoint

- `#[OA\Tag]` : groupement logique
- `#[OA\Parameter]` : chaque paramètre path et query
- `#[OA\RequestBody]` : schéma du body pour POST/PUT/PATCH
- `#[OA\Response]` : chaque code de réponse possible (200, 201, 400, 401, 403, 404, 422)

### Descriptions

- Summary : action en une phrase courte ("Crée un utilisateur", "Liste les commandes")
- Description : détails supplémentaires si nécessaire (pagination, filtres, comportement spécial)
- Les descriptions sont en français sauf si le projet utilise l'anglais

### Schémas

- Utiliser `#[Model(type: EntityDto::class)]` quand un DTO existe
- Définir le schéma inline quand il n'y a pas de DTO
- Documenter les champs required vs optionnels
- Documenter les formats (date-time, email, uuid)

### Réponses

Documenter systématiquement :
- `200` : succès (GET, PUT, PATCH)
- `201` : création réussie (POST)
- `204` : suppression réussie (DELETE)
- `400` : requête invalide (body malformé)
- `401` : non authentifié
- `403` : non autorisé
- `404` : ressource non trouvée
- `422` : erreurs de validation

### Exemple complet

```php
#[Route('/api/users/{id}', methods: ['GET'])]
#[OA\Tag(name: 'Users')]
#[OA\Parameter(name: 'id', in: 'path', schema: new OA\Schema(type: 'integer'))]
#[OA\Response(
    response: 200,
    description: 'Détails de l\'utilisateur',
    content: new OA\JsonContent(ref: new Model(type: UserDto::class))
)]
#[OA\Response(response: 404, description: 'Utilisateur non trouvé')]
public function show(int $id): JsonResponse
```

## Format de sortie

Générer les attributs OpenAPI directement au-dessus de chaque méthode du contrôleur. Le code doit être prêt à copier-coller.
