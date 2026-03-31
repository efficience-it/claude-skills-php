---
name: deprecations
description: "Scanne un projet PHP/Symfony pour détecter les deprecations PHP et Symfony. Identifie le code obsolète, propose les remplacements modernes."
---

# Détection des deprecations

## Deprecations PHP

### PHP 8.1
- `FILTER_SANITIZE_STRING`, `strftime()`, `utf8_encode()`/`utf8_decode()`

### PHP 8.2
- Propriétés dynamiques, `${var}` string interpolation, callables partiellement supportés

### PHP 8.3
- `NumberFormatter::TYPE_CURRENCY`

### PHP 8.4
- Constantes de classes sans visibilité explicite

## Deprecations Symfony

- Annotations au lieu d'attributs PHP 8
- `ContainerAwareTrait` / `ContainerAwareCommand`
- `AbstractController::getDoctrine()`
- `@Route` annotation au lieu de `#[Route]`
- `EventSubscriberInterface` remplacé par `#[AsEventListener]` (Symfony 6.2+)

### Commande de diagnostic
```bash
bin/console debug:container --deprecations
```

## Format de sortie

Par fichier : chemin, ligne, code déprécié, remplacement, version de suppression. Résumé avec nombre total, répartition par catégorie, priorité et estimation d'effort.
