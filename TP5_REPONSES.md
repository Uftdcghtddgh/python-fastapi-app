# Rendu TP5 – Construire un workflow CI avec GitHub Actions

**Étudiant :** Mohamed Ali Dakha  
**Dépôt GitHub :** [https://github.com/Uftdcghtddgh/python-fastapi-app](https://github.com/Uftdcghtddgh/python-fastapi-app)  
**Branche :** `feature/tp-ci`  

---

## Livrable 1 : Lien d'un run complet et vert

* **Lien direct du run CI complet et vert (Lint & Build $\rightarrow$ Test) :**  
  👉 [https://github.com/Uftdcghtddgh/python-fastapi-app/actions/runs/42284025594](https://github.com/Uftdcghtddgh/python-fastapi-app/actions/runs/42284025594)

* **Tableau de bord de tous les runs :**  
  [https://github.com/Uftdcghtddgh/python-fastapi-app/actions](https://github.com/Uftdcghtddgh/python-fastapi-app/actions)

---

## Livrable 2 : Réponses aux questions (Q0 à Q10)

* **Étape 0 (Q0) :**
  * *Code et tests :* Le code applicatif se trouve dans `app/` et les tests unitaires dans `tests/`.
  * *Dépendances de développement :* Listées dans le fichier `requirements-dev.txt`.
  * *Dossier .github :* Oui, il existe à la racine et contient le sous-dossier `workflows/`.

* **Q1 :** Configuré dans `.github/workflows/01-filtres.yml` avec `branches: [main, 'feature/**']`, `paths: ['app/**', 'tests/**']` et `pull_request: { types: [opened, synchronize] }`.

* **Q2 :** Modifier uniquement `README.md` ne lance pas le workflow car exclu du filtre `paths`. Modifier un fichier sous `app/` le lance automatiquement car il satisfait le filtre de chemin.

* **Q3 :** Entre `branches` et `paths`, la logique est un **ET** (les deux conditions doivent être réunies). Entre deux types d'activité d'un événement, la logique est un **OU**.

* **Q4 :** Workflow `tag.yml` configuré avec le filtre `tags: ['v*']`, testé et validé sur GitHub Actions avec la publication du tag `v1.0.1`.

* **Q5 :** Implémenté dans `.github/workflows/02-ci.yml` avec les jobs `lint` et `build` exécutés en parallèle, puis le job `test` dépendant de leur succès (`needs: [lint, build]`).

* **Q6 :** Sans `needs`, les trois jobs démarrent simultanément (durée totale = max des durées, mais gaspillage de ressources). Avec `needs`, si `lint` ou `build` échoue, le job `test` est ignoré (`skipped`).

* **Q7 :** Chaque job tourne dans une machine virtuelle indépendante (`ubuntu-latest`). L'environnement, le code extrait et les paquets Python installés ne sont pas partagés entre jobs.

* **Q8 :**
  * Nom de l'app : `${{ env.APP_NAME }}` (contexte `env`)
  * Acteur : `${{ github.actor }}` (contexte `github`)
  * Nom de l'événement : `${{ github.event_name }}` (contexte `github`)
  * Branche : `${{ github.ref_name }}` (contexte `github`)
  * SHA du commit : `${{ github.sha }}` (contexte `github`)
  * OS du runner : `${{ runner.os }}` (contexte `runner`)

* **Q9 :** Lors d'un `push`, `github.event_name` vaut `'push'` et `head_ref`/`base_ref` sont vides. Lors d'une `pull_request`, `github.event_name` devient `'pull_request'`, `head_ref` contient la branche source (`feature/tp-ci`) et `base_ref` contient la branche cible (`main`).

* **Q10 :** Dans l'objet JSON produit par `toJson(github)`, le message du commit se trouve dans la propriété `github.event.head_commit.message`.
