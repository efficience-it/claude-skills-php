---
name: command
description: "Génère une commande console Symfony. Produit une commande avec arguments, options, validation des inputs, progress bar, output structuré et gestion d'erreur. Utiliser quand on veut créer une commande CLI pour un projet Symfony."
---

# Génération de commande Symfony Console

## Processus

1. Comprendre l'objectif de la commande
2. Identifier les inputs nécessaires (arguments, options)
3. Générer la commande complète

## Structure

### Attribut PHP 8+ (Symfony 6.1+)

```php
#[AsCommand(
    name: 'app:entity:action',
    description: 'Description courte de la commande',
)]
```

### Nommage

- Préfixe `app:` pour les commandes applicatives
- Format : `app:domaine:action` (ex: `app:user:import`, `app:order:cleanup`)
- Verbe d'action explicite

### Arguments et options

```php
protected function configure(): void
{
    $this
        ->addArgument('file', InputArgument::REQUIRED, 'Chemin du fichier à importer')
        ->addOption('dry-run', null, InputOption::VALUE_NONE, 'Simule sans persister')
        ->addOption('limit', 'l', InputOption::VALUE_REQUIRED, 'Nombre max d\'éléments', '100')
    ;
}
```

### Execute

```php
protected function execute(InputInterface $input, OutputInterface $output): int
{
    $io = new SymfonyStyle($input, $output);

    // Validation des inputs
    // Logique (déléguée à un service)
    // Output structuré

    $io->success('Terminé.');
    return Command::SUCCESS;
}
```

## Règles

### Architecture

- La commande est un adaptateur mince : elle parse les inputs, appelle un service, et affiche le résultat
- Pas de logique métier dans la commande
- Injecter les dépendances par constructeur

### Validation des inputs

- Valider les arguments et options au début de `execute()`
- Vérifier l'existence des fichiers si un chemin est attendu
- Valider les formats (date, email, etc.)
- Retourner `Command::FAILURE` avec un message clair si invalide

### Output

- Utiliser `SymfonyStyle` pour un output propre
- `$io->title()` en début de commande
- `$io->section()` pour les sections
- `$io->table()` pour les données tabulaires
- `$io->progressBar()` ou `$io->createProgressBar()` pour les traitements longs
- `$io->success()` / `$io->error()` / `$io->warning()` pour le résultat final

### Traitement par batch

Pour les commandes qui traitent beaucoup de données :

```php
$progressBar = $io->createProgressBar($total);
$progressBar->start();

foreach ($items as $item) {
    // traitement
    $progressBar->advance();

    if ($count % 100 === 0) {
        $this->entityManager->flush();
        $this->entityManager->clear();
    }
}

$this->entityManager->flush();
$progressBar->finish();
```

### Dry-run

- Supporter `--dry-run` quand la commande modifie des données
- En mode dry-run : afficher ce qui serait fait sans persister
- Indiquer clairement dans l'output si c'est un dry-run

### Codes de retour

- `Command::SUCCESS` (0) : tout s'est bien passé
- `Command::FAILURE` (1) : erreur
- `Command::INVALID` (2) : arguments invalides

## Format de sortie

Générer la classe de commande complète avec imports, attribut, configuration, et méthode execute. La commande doit être prête à fonctionner.
