---
name: security
description: "Audit de sécurité rapide pour projet PHP/Symfony. Analyse les fichiers modifiés ou un périmètre donné. Détecte injections SQL, XSS, CSRF, exposition de données, permissions manquantes. Basé sur OWASP Top 10. Utiliser avant un commit ou pour auditer un périmètre."
---

# Audit de sécurité PHP/Symfony

## Périmètre

Par défaut, analyse les fichiers modifiés (`git diff`). Si l'utilisateur précise un fichier, un dossier ou un périmètre, analyser celui-ci.

## Critères d'analyse (OWASP Top 10)

### A01 - Broken Access Control (bloquant)

- Contrôleur sans `#[IsGranted]` ni `denyAccessUnlessGranted()`
- Route sensible accessible sans authentification
- Vérification d'accès basée sur l'ID utilisateur passé en paramètre (IDOR)
- Voter absent pour les opérations sur des entités avec propriétaire
- `security.yaml` : firewall avec `security: false` en production
- Accès direct à une entité sans vérifier que l'utilisateur courant y a droit

### A02 - Cryptographic Failures (bloquant)

- Mot de passe stocké en clair ou hashé avec MD5/SHA1
- Token/secret en dur dans le code source
- Fichier `.env` avec des secrets committé
- Clé de chiffrement faible ou prévisible
- Communication HTTP au lieu de HTTPS pour des données sensibles

### A03 - Injection (bloquant)

- Requête DQL/SQL construite par concaténation de variables
- `$connection->executeQuery("SELECT ... WHERE id = $id")` sans binding
- Commande shell construite avec des données utilisateur sans `escapeshellarg()`
- Expression régulière construite avec des données utilisateur
- Requête LDAP sans échappement

### A04 - Insecure Design (important)

- Absence de rate limiting sur login, reset password, API publique
- Pas de validation côté serveur (uniquement côté client)
- Logique d'autorisation dans le template Twig au lieu du contrôleur/voter
- Endpoint qui retourne plus de données que nécessaire (sur-exposition)

### A05 - Security Misconfiguration (important)

- `APP_DEBUG=true` en production
- `kernel.debug` activé en prod
- Profiler Symfony accessible en production
- CORS trop permissif (`allow_origin: '*'`)
- Headers de sécurité manquants (CSP, X-Frame-Options, HSTS)

### A07 - XSS (bloquant)

- `|raw` sur des données utilisateur dans Twig
- `{{ variable|raw }}` sans sanitization préalable
- Injection dans des attributs HTML (`href="{{ user_input }}"`)
- Données utilisateur dans du JavaScript inline

### A08 - Insecure Deserialization (bloquant)

- `unserialize()` sur des données utilisateur
- Désérialisation JSON sans validation de schéma avant traitement
- `Serializer::deserialize()` sans groupes de dénormalisation

### Fichiers sensibles

- `.env`, `.env.local` avec des secrets dans le git
- `composer.lock` absent du repository (versions non verrouillées)
- Fichiers de configuration avec des credentials

## Format de sortie

Pour chaque vulnérabilité :
- Fichier et ligne
- Catégorie OWASP
- Sévérité (bloquant / important / suggestion)
- Code vulnérable
- Code corrigé
- Référence (lien doc Symfony Security si applicable)
