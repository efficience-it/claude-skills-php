---
name: dto
description: "Génère des DTOs (Data Transfer Objects) pour projet PHP/Symfony. Produit des classes readonly avec validation, mapping depuis/vers les entités Doctrine."
---

# Génération de DTOs

## Types de DTO

### Input DTO (création/modification)
- Classes `readonly`, promotion de propriétés dans le constructeur
- Contraintes de validation (`#[Assert\...]`)
- Ne contient que les champs modifiables par le client

### Output DTO (lecture)
- Pas de contraintes de validation
- Peut contenir des champs calculés ou formatés
- Méthode statique `fromEntity()` pour le mapping

### Command/Query DTO (CQRS)
- Command : intention de modification
- Query : demande de données
- Immutables (readonly)

## Règles

- Classes `readonly` (PHP 8.2+)
- Pas de logique métier dans le DTO
- Pas de dépendance vers Doctrine ou Symfony (sauf Assert)
- Nommage : `Create{Entity}Dto`, `Update{Entity}Dto`, `{Entity}Dto`

## Format de sortie

Générer les classes DTO complètes avec imports, validation, et méthodes de mapping.
