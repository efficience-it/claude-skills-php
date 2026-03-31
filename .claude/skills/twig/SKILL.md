---
name: twig
description: "Audit et amélioration de templates Twig. Vérifie l'accessibilité, la performance, les bonnes pratiques, et la sécurité. Utiliser pour auditer un template existant ou quand on veut un retour sur la qualité d'un template."
---

# Audit de templates Twig

## Périmètre

Analyser le template indiqué ou les templates du dossier `templates/`. Si aucun périmètre n'est précisé, se concentrer sur les templates modifiés (`git diff`).

## Critères d'analyse

### Sécurité (bloquant)

- `|raw` sur des données utilisateur sans sanitization
- Variables dans des attributs HTML sans échappement contextuel
- URLs construites à partir de données utilisateur (`href="{{ user_url }}"`)
- Formulaires sans token CSRF (`{{ csrf_token('intention') }}` ou `form_rest()`)
- Inclusion de template dynamique (`include(variable)`) avec des données non fiables

### Performance (important)

- Requêtes Doctrine déclenchées dans le template (lazy loading via des getters de relation)
- Boucles sur des collections non paginées
- Filtres coûteux dans des boucles (`|sort`, `|filter`, `|map` sur de grandes collections)
- Assets non versionnés (cache busting absent)
- Blocs répétitifs qui pourraient être cachés avec `{% cache %}`

### Architecture (important)

- Logique métier dans le template (calculs, conditions complexes, formatage avancé)
  - Extraire vers une Twig Extension ou un service
- Template de plus de 200 lignes sans extraction de blocs ou partials
- Duplication de HTML entre templates
  - Extraire dans un `_partial.html.twig` ou un Twig Component

### Accessibilité (important)

- Images sans attribut `alt`
- Formulaires sans labels associés aux inputs
- Liens sans texte descriptif (`<a href="..."><img></a>` sans alt ni aria-label)
- Hiérarchie de headings cassée (h1 puis h3 sans h2)
- Contraste insuffisant si des styles inline sont présents
- Éléments interactifs non accessibles au clavier (onclick sans role/tabindex)
- Tableaux sans `<thead>`, `<th>`, ou `scope`

### Bonnes pratiques Twig

- Utiliser `{{ path('route_name') }}` au lieu de URLs en dur
- Utiliser `{{ asset('path') }}` pour les fichiers statiques
- Utiliser `|trans` pour les chaînes (i18n-ready)
- Préférer les Twig Components aux macros pour les éléments réutilisables
- `form_rest(form)` à la fin de chaque formulaire (champs cachés, CSRF)
- Nommage des blocs : explicite et cohérent (`{% block page_title %}`, pas `{% block t %}`)

### Structure des templates

- Héritage : un template de base (`base.html.twig`), des layouts intermédiaires si nécessaire
- Nommage : `entity/action.html.twig` cohérent avec les routes
- Partials préfixés par `_` : `_navbar.html.twig`, `_sidebar.html.twig`

## Format de sortie

Pour chaque problème :
- Fichier et ligne
- Catégorie (sécurité, performance, accessibilité, bonnes pratiques)
- Sévérité
- Code problématique
- Code corrigé

Résumé en fin d'audit avec le nombre de problèmes par catégorie.
