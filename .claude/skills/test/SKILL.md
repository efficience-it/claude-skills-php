---
name: test
description: "Génère des tests PHPUnit pour un fichier ou une classe PHP/Symfony. Couvre le cas nominal, les cas limites et les cas d'erreur. Respecte les conventions du projet (Unit/Functional, data providers, pas de mock DB en fonctionnel). Utiliser quand un développeur veut des tests pour du code existant ou nouveau."
---

# Génération de tests PHPUnit

## Processus

1. Lire le fichier cible pour comprendre la logique
2. Identifier le type de test adapté (unitaire ou fonctionnel)
3. Analyser les conventions du projet (structure tests/, nommage, base classes)
4. Générer le test complet

## Conventions à détecter

Avant de générer, inspecter le dossier `tests/` pour identifier :

- Structure : `tests/Unit/`, `tests/Functional/`, `tests/Integration/`, ou autre
- Base class utilisée : `TestCase`, `KernelTestCase`, `WebTestCase`, custom
- Nommage des méthodes : `test_snake_case()`, `testCamelCase()`, ou attribut `#[Test]`
- Utilisation de data providers : `#[DataProvider('providerName')]`
- Setup/Teardown patterns du projet

## Règles

### Tests unitaires (classes Domain, ValueObjects, Services sans dépendance externe)

- Placer dans `tests/Unit/` en miroir de la structure `src/`
- Aucune dépendance au kernel Symfony
- Mocker les interfaces, pas les classes concrètes
- Couvrir :
  - Le cas nominal (happy path)
  - Au moins un cas limite (null, vide, valeur extrême)
  - Au moins un cas d'erreur (exception attendue)

### Tests fonctionnels (contrôleurs, commandes, intégration)

- Placer dans `tests/Functional/` ou `tests/Integration/`
- Ne jamais mocker la base de données
- Utiliser les fixtures du projet si elles existent
- Tester les codes de réponse HTTP, le contenu, les redirections

### Data providers

Utiliser un data provider quand :
- Plus de 2 cas testent la même méthode avec des entrées différentes
- Les cas limites sont nombreux (validation, parsing, conversion)

Format :
```php
#[DataProvider('exampleProvider')]
public function test_example(string $input, string $expected): void
{
    // ...
}

public static function exampleProvider(): iterable
{
    yield 'cas nominal' => ['input', 'expected'];
    yield 'cas limite' => ['', ''];
}
```

### Assertions

- Préférer les assertions spécifiques (`assertSame` > `assertEquals` > `assertTrue`)
- Un test = un comportement. Pas 15 assertions dans un seul test.
- Nommer les tests de manière descriptive : `test_create_user_with_invalid_email_throws_exception`

## Format de sortie

Générer le fichier de test complet, prêt à exécuter. Inclure les imports, la classe, et toutes les méthodes de test.
