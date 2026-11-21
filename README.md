# TP CI/CD avec GitHub Actions — Application Node.js

Ce dépôt contient le projet réalisé dans le cadre du TP DevOps : automatisation d'un pipeline CI/CD multi-jobs avec GitHub Actions pour une application Node.js Express.

## Questions de synthèse

### 1. Définissez les notions de workflow, job, step et runner dans GitHub Actions.
- **Workflow** : Processus automatisé configurable composé d'un ou plusieurs jobs, défini dans un fichier YAML stocké dans le dossier `.github/workflows/` et déclenché par un ou des événements (ex. `push`, `pull_request`).
- **Job** : Ensemble d'étapes (steps) exécutées séquentiellement sur le même runner. Par défaut, les jobs d'un workflow s'exécutent en parallèle, sauf si des dépendances sont définies via `needs`.
- **Step** : Tâche unitaire et séquentielle au sein d'un job. Il peut s'agir d'une action réutilisable (`uses`) ou d'une commande shell (`run`).
- **Runner** : Machine virtuelle ou conteneur (ex. `ubuntu-latest`) fourni par GitHub ou auto-hébergé, sur lequel s'exécutent les jobs.

### 2. Quelle est la différence entre `uses` et `run` dans une step ?
- **`uses`** : Permet d'appeler et d'exécuter une **action préexistante** et réutilisable (issue du GitHub Marketplace ou de la communauté, comme `actions/checkout@v4`).
- **`run`** : Permet d'exécuter une **commande shell directe** sur la machine hôte du runner (comme `npm test`, `ls` ou `echo`).

### 3. À quoi sert `needs` ? Comment s'exécutent des jobs qui n'en déclarent pas ?
- **`needs`** : Permet de définir des **dépendances** entre jobs afin d'orchestrer leur ordre d'exécution (ex. `job-4` attend la réussite de `job-2` et `job-3`). Si un job parent échoue, les jobs dépendants sont ignorés (`skipped`).
- Les jobs qui ne déclarent aucun `needs` s'exécutent **en parallèle** dès le déclenchement du workflow.

### 4. Pourquoi déclencher le pipeline sur `pull_request` en plus de `push` ?
- Déclencher le pipeline sur `pull_request` permet de tester et valider le code d'une branche avant qu'il ne soit fusionné (*merged*) dans la branche principale (`main`).
- Cela empêche d'introduire des régressions, des bugs ou du code défaillant sur la branche de production.

---

## Questions intermédiaires du TP

- **Différence entre CI, livraison continue et déploiement continu** :
  - *CI (Intégration continue)* : Automatisation des tests et du build à chaque commit.
  - *Livraison continue* : Déploiement automatique jusqu'à un environnement de pré-production, avec validation manuelle pour la mise en production.
  - *Déploiement continu* : Déploiement automatique en production dès que les tests CI sont validés.
- **Pourquoi `node_modules` ne doit pas être versionné alors que `package-lock.json` le doit** :
  - `node_modules` contient des dépendances volumineuses régénérables avec `npm install` et pouvant dépendre de l'OS.
  - `package-lock.json` fige les versions exactes de l'arbre des dépendances pour garantir la reproductibilité sur tous les environnements.
- **Rôle de `actions/checkout`** :
  - Clone le code du dépôt sur le runner. Sans cette étape, le runner est vierge et les fichiers du projet ne sont pas accessibles.
- **Comportement lors de l'exécution #3 (échec de job-3)** :
  - `job-2` a réussi car il ne dépendait que de `job-1`.
  - `job-4` a été ignoré (`skipped`) car il attendait le succès de `job-3` (`needs: [job-2, job-3]`).
