# TP DevOps - GitHub Actions

Projet Node.js / Express avec mise en place d'un pipeline CI/CD multi-jobs sur GitHub Actions.

## Réponses aux questions

### 1. Workflow, Job, Step et Runner
- **Workflow** : Le fichier de configuration YAML dans `.github/workflows/` qui définit tout le pipeline et ses déclencheurs (push, pull_request).
- **Job** : Un groupe d'étapes qui tourne sur une même machine. Par défaut, les jobs s'exécutent en parallèle.
- **Step** : Une étape individuelle dans un job, soit une commande bash, soit une action réutilisable.
- **Runner** : La machine virtuelle (ubuntu-latest) fournie par GitHub pour exécuter le job.

### 2. Différence entre `uses` et `run`
- **`uses`** : Utilise une action préfaite disponible sur GitHub (ex: `actions/checkout@v4` pour récupérer le code).
- **`run`** : Exécute une commande terminal directement sur le runner (ex: `ls`, `npm start`, etc.).

### 3. Utilité de `needs`
- `needs` permet de créer des dépendances entre les jobs (ex: job-4 attend la réussite de job-2 et job-3).
- Sans `needs`, tous les jobs démarrent en même temps en parallèle.

### 4. Déclenchement sur `pull_request` et `push`
Déclencher sur `pull_request` permet de vérifier que les tests passent et que le code fonctionne avant de valider la fusion (merge) sur la branche `main`.
