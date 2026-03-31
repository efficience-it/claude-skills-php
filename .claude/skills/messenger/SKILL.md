---
name: messenger
description: "Vérifie la configuration Symfony Messenger. Analyse les transports, routages, handlers, retry policies et middlewares. Détecte les messages sans handler et les incohérences."
---

# Vérification Symfony Messenger

## Processus

1. Lire la configuration Messenger (`config/packages/messenger.yaml` ou `.php`)
2. Scanner tous les messages et handlers
3. Croiser les informations et détecter les incohérences

## Critères d'analyse

### Routage (bloquant)

- Message sans handler enregistré
- Handler enregistré pour un message qui n'existe pas
- Message routé vers un transport qui n'est pas défini
- Message synchrone qui devrait être asynchrone (traitement long, appel externe)
- Message asynchrone sans transport explicite (fallback sur sync silencieux)

### Retry et failure (important)

- Transport async sans retry policy définie
- Retry infini (pas de `max_retries`)
- Absence de failure transport pour les transports async

### Handlers (important)

- Handler qui contient de la logique métier lourde (> 50 lignes)
- Handler qui fait des appels HTTP synchrones sans timeout
- Handler qui flush Doctrine dans une boucle
- Handler sans gestion d'erreur pour les cas d'échec récupérables

### Serialization

- Transport externe (RabbitMQ, SQS) sans serializer explicite
- Serializer PHP natif sur un transport partagé entre applications

## Format de sortie

1. Résumé : nombre de messages, handlers, transports
2. Matrice de routage : quel message va sur quel transport
3. Problèmes détectés classés par sévérité
4. Corrections proposées avec exemples de configuration
