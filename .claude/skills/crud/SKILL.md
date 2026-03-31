---
name: crud
description: "Génère un CRUD complet pour une entité Symfony. Produit l'entité Doctrine, le repository, le contrôleur, le form type, les templates Twig, et les tests. Utiliser quand on veut scaffolder rapidement une ressource complète."
---

# Génération de CRUD Symfony

## Processus

1. Comprendre l'entité demandée (champs, types, relations, contraintes)
2. Détecter l'architecture du projet (MVC, hexagonal, API-only)
3. Générer tous les fichiers nécessaires dans le bon ordre

## Détection du contexte

- Templates Twig présents ? -> CRUD web classique
- API Platform installé ? -> Resource API Platform
- Uniquement des contrôleurs JSON ? -> CRUD API sans Twig
- Architecture hexagonale ? -> Séparer Domain/Application/Infrastructure

## Fichiers générés

### 1. Entité Doctrine

- Attributs PHP 8+ (`#[ORM\Entity]`, `#[ORM\Column]`)
- Contraintes de validation (`#[Assert\NotBlank]`, etc.)
- Relations si demandées
- Méthodes métier (pas de setters aveugles si architecture hexagonale)

### 2. Repository

- Étend `ServiceEntityRepository`
- Méthodes `save()` et `remove()` avec flush optionnel
- Méthodes de recherche spécifiques si identifiables

### 3. Contrôleur

- Routes avec attributs PHP 8+ (`#[Route]`)
- Actions : `index`, `show`, `new`, `edit`, `delete`
- `#[IsGranted]` sur chaque action
- Flash messages en français
- Redirection après création/modification/suppression

### 4. Form Type

- Champs typés avec les bons FormType (TextType, DateType, EntityType, etc.)
- Labels en français
- Contraintes de validation cohérentes avec l'entité
- Options (placeholder, required, help)

### 5. Templates Twig (si web)

- `index.html.twig` : liste paginée avec tableau
- `show.html.twig` : détail de l'entité
- `new.html.twig` : formulaire de création
- `edit.html.twig` : formulaire de modification
- `_form.html.twig` : partial formulaire partagé entre new et edit
- `_delete_form.html.twig` : formulaire de suppression avec confirmation

### 6. Tests

- Test unitaire de l'entité (validation, méthodes métier)
- Test fonctionnel du contrôleur (chaque route, codes HTTP, redirections)

## Conventions

- Nommage des routes : `app_entity_action` (ex: `app_user_index`, `app_user_new`)
- Nommage des templates : `entity/action.html.twig`
- Messages flash : `success`, `error`
- Pagination : KnpPaginatorBundle si installé, sinon pagination manuelle

## Format de sortie

Générer chaque fichier avec son chemin complet, prêt à copier dans le projet. Indiquer l'ordre de création (entité en premier, puis migration, puis le reste).
