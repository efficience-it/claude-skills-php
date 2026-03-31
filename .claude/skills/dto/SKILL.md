---
name: dto
description: "Génère des DTOs (Data Transfer Objects) pour projet PHP/Symfony. Produit des classes readonly avec validation, mapping depuis/vers les entités Doctrine. Utiliser quand on veut découpler la couche API ou formulaire des entités."
---

# Génération de DTOs

## Processus

1. Lire l'entité source ou comprendre la structure demandée
2. Identifier le contexte d'utilisation (API input, API output, form, command, query)
3. Générer le DTO avec validation et mapping

## Types de DTO

### Input DTO (création/modification)

- Reçoit les données de l'extérieur (requête HTTP, formulaire, commande)
- Contient les contraintes de validation (`#[Assert\...]`)
- Ne contient que les champs modifiables par le client
- Pas d'ID (c'est le serveur qui le génère)

```php
final readonly class CreateUserDto
{
    public function __construct(
        #[Assert\NotBlank]
        #[Assert\Email]
        public string $email,

        #[Assert\NotBlank]
        #[Assert\Length(min: 2, max: 100)]
        public string $firstName,

        #[Assert\NotBlank]
        #[Assert\Length(min: 2, max: 100)]
        public string $lastName,
    ) {}
}
```

### Output DTO (lecture)

- Expose les données vers l'extérieur (réponse API, vue)
- Pas de contraintes de validation
- Peut contenir des champs calculés ou formatés
- Contient l'ID

```php
final readonly class UserDto
{
    public function __construct(
        public int $id,
        public string $email,
        public string $fullName,
        public string $createdAt,
    ) {}

    public static function fromEntity(User $user): self
    {
        return new self(
            id: $user->getId(),
            email: $user->getEmail(),
            fullName: $user->getFirstName() . ' ' . $user->getLastName(),
            createdAt: $user->getCreatedAt()->format('Y-m-d'),
        );
    }
}
```

### Command/Query DTO (CQRS)

- Command : représente une intention de modification
- Query : représente une demande de données
- Immutables (readonly)

## Règles

### Structure

- Classes `readonly` (PHP 8.2+)
- Promotion de propriétés dans le constructeur
- Pas de logique métier dans le DTO
- Pas de dépendance vers Doctrine ou Symfony (sauf Assert)

### Nommage

- Input : `Create{Entity}Dto`, `Update{Entity}Dto`
- Output : `{Entity}Dto`, `{Entity}ListItemDto`
- Command : `Create{Entity}Command`, `Update{Entity}Command`
- Query : `Get{Entity}Query`, `List{Entity}Query`

### Mapping

- `fromEntity()` : méthode statique sur le DTO output pour convertir depuis l'entité
- `toEntity()` : méthode sur le DTO input si la création est simple
- Pour les cas complexes, utiliser un mapper/factory dédié

### Validation

- Les contraintes de validation sont sur le DTO input, pas sur l'entité
- Utiliser les groupes de validation si un DTO sert à plusieurs contextes
- Valider les relations par ID (`#[Assert\Positive]` sur `categoryId`) plutôt que par entité

### Collections

- Pour les listes, créer un DTO dédié avec pagination :

```php
final readonly class UserListDto
{
    public function __construct(
        /** @var UserDto[] */
        public array $items,
        public int $total,
        public int $page,
        public int $limit,
    ) {}
}
```

## Format de sortie

Générer les classes DTO complètes avec imports, validation, et méthodes de mapping. Indiquer dans quel namespace/dossier placer chaque fichier.
