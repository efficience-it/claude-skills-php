---
name: fixtures
description: "Génère des fixtures de test pour les entités Doctrine. Produit des données réalistes avec Alice/Faker, gère les relations, respecte les contraintes de validation. Utiliser quand on a besoin de jeux de données pour les tests fonctionnels."
---

# Génération de fixtures

## Processus

1. Lire l'entité cible et ses annotations/attributs Doctrine
2. Identifier les contraintes de validation (`#[Assert\...]`)
3. Détecter les relations (ManyToOne, OneToMany, ManyToMany)
4. Déterminer le format utilisé dans le projet (Alice YAML, PHP fixtures, DoctrineFixturesBundle)
5. Générer les fixtures

## Détection du format

Inspecter le projet pour identifier le système de fixtures en place :

- `fixtures/*.yaml` ou `*.yml` : Alice (hautelook/alice-bundle ou fidry/alice-data-fixtures)
- `src/DataFixtures/*.php` : DoctrineFixturesBundle
- `tests/fixtures/` ou `tests/DataFixtures/` : fixtures de test dédiées

Utiliser le format existant. Si aucun n'est en place, proposer DoctrineFixturesBundle.

## Règles

### Données réalistes

- Noms et prénoms français (pas de "John Doe")
- Emails valides et cohérents avec les noms
- Dates dans des plages réalistes (pas de dates en l'an 3000)
- Numéros de téléphone au format français
- Adresses françaises
- SIRET, TVA intracommunautaire si pertinent

### Couverture

- Minimum 5 entrées par entité
- Varier les cas : valeurs normales, valeurs aux limites, cas spéciaux
- Inclure au moins un cas avec des champs optionnels à null
- Inclure au moins un cas avec des valeurs longues (proches des limites VARCHAR)

### Relations

- Créer les entités référencées en premier
- Utiliser des références nommées de manière explicite
- Couvrir les cas : relation présente, relation nulle (si nullable), relation multiple

### Contraintes de validation

- Chaque fixture doit respecter les contraintes `#[Assert\...]` de l'entité
- Créer aussi des fixtures invalides (dans un groupe séparé) pour tester la validation

## Format Alice (YAML)

```yaml
App\Entity\User:
    user_admin:
        email: 'admin@example.com'
        firstName: 'Marie'
        lastName: 'Dupont'
        roles: ['ROLE_ADMIN']
    user_{1..5}:
        email: '<email()>'
        firstName: '<firstNameFemale()>'
        lastName: '<lastName()>'
        roles: ['ROLE_USER']
```

## Format DoctrineFixturesBundle (PHP)

```php
public function load(ObjectManager $manager): void
{
    $user = new User();
    $user->setEmail('admin@example.com');
    $user->setFirstName('Marie');
    $user->setLastName('Dupont');
    $manager->persist($user);

    $this->addReference('user_admin', $user);
    $manager->flush();
}
```

## Format de sortie

Générer le fichier de fixtures complet, prêt à utiliser, avec les références nécessaires pour les relations.
