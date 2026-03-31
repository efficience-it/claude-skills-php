---
name: api-platform
description: "Génère une ressource API Platform pour projet Symfony. Configure les opérations, filtres, pagination, groupes de sérialisation, et validations. Utiliser quand on travaille avec API Platform pour exposer ou modifier une ressource API."
---

# Ressource API Platform

## Processus

1. Comprendre l'entité ou le DTO à exposer
2. Détecter la version d'API Platform (2.x ou 3.x+)
3. Configurer la ressource avec les bonnes opérations et options

## Détection de la version

- `composer.json` : version de `api-platform/core`
- API Platform 3.x+ : attributs `#[ApiResource]` avec `operations: []`
- API Platform 2.x : attributs avec `itemOperations` / `collectionOperations`

## Configuration de la ressource

### Attribut ApiResource (3.x+)

```php
#[ApiResource(
    operations: [
        new GetCollection(),
        new Get(),
        new Post(
            validationContext: ['groups' => ['create']],
            security: "is_granted('ROLE_ADMIN')",
        ),
        new Put(
            security: "is_granted('ROLE_ADMIN') or object.owner == user",
        ),
        new Delete(
            security: "is_granted('ROLE_ADMIN')",
        ),
    ],
    normalizationContext: ['groups' => ['read']],
    denormalizationContext: ['groups' => ['write']],
    paginationItemsPerPage: 30,
)]
```

### Groupes de sérialisation

- `#[Groups(['read'])]` : champs exposés en lecture (GET)
- `#[Groups(['write'])]` : champs acceptés en écriture (POST/PUT)
- `#[Groups(['read', 'write'])]` : les deux
- Créer des groupes spécifiques pour les listes vs le détail (`list`, `detail`)

```php
#[ORM\Column]
#[Groups(['read', 'write'])]
#[Assert\NotBlank(groups: ['create'])]
private string $name;

#[ORM\Column]
#[Groups(['detail'])]
private \DateTimeImmutable $createdAt;
```

### Filtres

```php
#[ApiFilter(SearchFilter::class, properties: [
    'name' => 'partial',
    'email' => 'exact',
    'status' => 'exact',
])]
#[ApiFilter(DateFilter::class, properties: ['createdAt'])]
#[ApiFilter(OrderFilter::class, properties: ['name', 'createdAt'])]
#[ApiFilter(BooleanFilter::class, properties: ['active'])]
```

### Pagination

- Défaut : 30 items par page
- Client-side : `paginationClientItemsPerPage: true` pour permettre `?itemsPerPage=50`
- Max : `paginationMaximumItemsPerPage: 100`
- Désactiver : `paginationEnabled: false` (uniquement pour les petites collections)

### Validation

- Utiliser les groupes de validation pour différencier create/update
- Ajouter les contraintes directement sur l'entité ou le DTO
- `validationContext: ['groups' => ['create']]` sur l'opération Post

### Sécurité

- `security` : expression de sécurité par opération
- `securityPostDenormalize` : vérification après hydratation (pour comparer ancien/nouveau)
- Utiliser les voters pour la logique complexe

### State Providers / State Processors (3.x+)

Pour les cas qui ne sont pas du CRUD Doctrine standard :

- Custom Provider : pour des sources de données externes ou des requêtes complexes
- Custom Processor : pour une logique de persistance custom

```php
#[ApiResource(
    provider: CustomProvider::class,
    processor: CustomProcessor::class,
)]
```

### Relations

- Relations exposées par IRI par défaut (`/api/categories/1`)
- Pour embarquer les données : ajouter le groupe de sérialisation sur la relation
- `#[ApiSubresource]` pour les sous-collections (`/api/users/1/orders`)

## Format de sortie

Générer l'entité ou le DTO avec tous les attributs API Platform, prêt à utiliser. Inclure les filtres, groupes, et configuration de sécurité adaptés au contexte.
