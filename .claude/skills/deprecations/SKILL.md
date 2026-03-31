---
name: deprecations
description: "Scanne un projet PHP/Symfony pour détecter les deprecations PHP et Symfony. Identifie le code obsolète, propose les remplacements modernes. Utiliser pour préparer une montée de version ou réduire la dette technique."
---

# Détection des deprecations

## Processus

1. Identifier les versions PHP et Symfony du projet
2. Scanner le code source pour les patterns dépréciés
3. Proposer les remplacements

## Deprecations PHP

### PHP 8.0 deprecations (si encore utilisées)

- `create_function()` -> closures
- `each()` -> `foreach`
- Paramètre `$sort_flags` par référence dans `sort()` et similaires

### PHP 8.1 deprecations

- `FILTER_SANITIZE_STRING` -> `htmlspecialchars()`
- `strftime()` -> `IntlDateFormatter`
- `utf8_encode()` / `utf8_decode()` -> `mb_convert_encoding()`
- `$GLOBALS` en écriture
- Return type declarations manquantes sur les méthodes internes surchargées

### PHP 8.2 deprecations

- Propriétés dynamiques (sans `#[AllowDynamicProperties]`)
- `${var}` string interpolation -> `{$var}`
- `strtolower()` / `strtoupper()` dépréciés pour les locales non-ASCII
- Callables partiellement supportés (`"self::method"`, `[Parent::class, 'method']`)

### PHP 8.3 deprecations

- `NumberFormatter::TYPE_CURRENCY`
- Passer un nombre négatif à `mb_strimwidth()`
- `ldap_connect()` avec deux paramètres

### PHP 8.4 deprecations

- `implode()` avec les arguments inversés
- Constantes de classes sans visibilité explicite
- `GET`/`POST`/`COOKIE`/`SERVER` accès direct dépréciés dans certains contextes

## Deprecations Symfony

### Détection automatique

Scanner pour les patterns courants selon la version du projet :

- Services publics par défaut (< Symfony 4)
- Annotations au lieu d'attributs PHP 8 (< Symfony 6.2)
- `TreeBuilder` sans `getRootNode()` (< Symfony 4.3)
- `ContainerAwareTrait` / `ContainerAwareCommand`
- `AbstractController::getDoctrine()`
- `security.yaml` avec `enable_authenticator_manager`
- `@Route` annotation au lieu de `#[Route]`
- `AbstractType::configureOptions` sans type hint
- Accès au container depuis les commandes
- `EventSubscriberInterface` remplacé par `#[AsEventListener]` (Symfony 6.2+)

### Commande de diagnostic

Suggérer de lancer :
```bash
bin/console debug:container --deprecations
```

Pour les deprecations détectées au runtime.

## Format de sortie

### Par fichier

Pour chaque fichier avec des deprecations :
- Chemin du fichier
- Ligne et code déprécié
- Remplacement recommandé
- Version PHP/Symfony où ce sera supprimé

### Résumé

- Nombre total de deprecations
- Répartition par catégorie (PHP / Symfony / bundles tiers)
- Priorité : ce qui sera supprimé dans la prochaine version majeure en premier
- Estimation de l'effort de correction (trivial / modéré / complexe)
