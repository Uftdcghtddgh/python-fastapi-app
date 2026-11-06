# Rendu TP5 – Construire un workflow CI avec GitHub Actions

> **Étudiant :** Mohamed Ali Dakha  
> **Dépôt GitHub :** [https://github.com/Uftdcghtddgh/python-fastapi-app](https://github.com/Uftdcghtddgh/python-fastapi-app)  
> **Branche de travail :** `feature/tp-ci`  
> **Pull Request :** [PR #1](https://github.com/Uftdcghtddgh/python-fastapi-app/pull/1)  

---

## 🎯 Livrable 1 : Lien du run complet et vert (Graphe Lint & Build → Test)

👉 **Run CI vert validé :** [https://github.com/Uftdcghtddgh/python-fastapi-app/actions/runs/42284025594](https://github.com/Uftdcghtddgh/python-fastapi-app/actions/runs/42284025594)

Tous les workflows sont validés et verts sur GitHub Actions :
* `02-ci.yml` : Jobs `lint` + `build` en parallèle $\rightarrow$ `test` (needs `[lint, build]`).
* `01-filtres.yml` : Filtres de branches et chemins (`paths`), événements `push` et `pull_request`.
* `tag.yml` : Déclenché sur les tags `v*` (validé avec `v1.0.1`).
* `03-contextes.yml` : Affichage de `APP_NAME`, `github.actor`, `github.event_name`, `github.ref_name`, `github.sha`, `runner.os` et `toJson(github)`.

---

## 📝 Livrable 2 : Réponses aux questions (Q0 à Q10)

* **Étape 0 (Q0) :**
  * *Emplacement du code et des tests :* Le code est dans `app/` et les tests unitaires dans `tests/`.
  * *Dépendances de développement :* Listées dans `requirements-dev.txt`.
  * *Dossier .github :* Oui, il existe à la racine et contient le dossier `workflows/`.

* **Q1 :** Configuré dans `.github/workflows/01-filtres.yml` avec `branches: [main, 'feature/**']`, `paths: ['app/**', 'tests/**']` et `pull_request: { types: [opened, synchronize] }`.

* **Q2 :** Modifier uniquement `README.md` ne déclenche pas le workflow car le fichier est hors du filtre `paths`. Modifier un fichier sous `app/` déclenche immédiatement le workflow car le filtre est satisfait.

* **Q3 :** Entre `branches` et `paths`, la logique est un **ET** (les deux conditions doivent être vraies simultanément). Entre deux types d'activité d'un événement, la logique est un **OU** (l'une ou l'autre déclenche).

* **Q4 :** Configuré dans `.github/workflows/tag.yml` avec `tags: ['v*']`. Validé avec succès en créant et poussant le tag `v1.0.1`.

* **Q5 :** Implémenté dans `.github/workflows/02-ci.yml` avec les jobs `lint` et `build` en parallèle, puis le job `test` exécuté après via `needs: [lint, build]`.

* **Q6 :** Sans `needs`, les trois jobs s'exécutent en parallèle (durée égale au job le plus long, mais gaspillage de ressources). Avec `needs`, si `lint` échoue, le job `test` est ignoré (`skipped`).

* **Q7 :** Chaque job s'exécute dans un runner dédié (machine virtuelle isolée). Le code extrait et les paquets installés ne sont pas conservés d'un job à l'autre.

* **Q8 :**
  * Nom de l'app : `${{ env.APP_NAME }}` (contexte `env`)
  * Acteur : `${{ github.actor }}` (contexte `github`)
  * Nom de l'événement : `${{ github.event_name }}` (contexte `github`)
  * Branche : `${{ github.ref_name }}` (contexte `github`)
  * SHA du commit : `${{ github.sha }}` (contexte `github`)
  * OS du runner : `${{ runner.os }}` (contexte `runner`)

* **Q9 :** Lors d'un `push`, `github.event_name` vaut `'push'`, `github.head_ref` et `github.base_ref` sont vides. Lors d'une `pull_request`, `github.event_name` devient `'pull_request'`, `github.head_ref` vaut la branche source (`feature/tp-ci`) et `github.base_ref` vaut la branche cible (`main`).

* **Q10 :** Dans le dump JSON produit par `${{ toJson(github) }}`, le message du commit se trouve dans la propriété `github.event.head_commit.message`.

---

# FastAPI Starter (Documentation du Projet)

A minimal, production‑leaning FastAPI template with testing (coverage), linting/formatting, Docker, and CI. Ships with a root endpoint and a health check.

## Features
- FastAPI + Uvicorn
- Pydantic Settings for configuration management
- Custom logging with TRACE level support
- Testing with pytest + pytest-cov (HTML/XML coverage, 85% threshold)
- Ruff for linting and formatting; optional mypy typing
- Dockerfile and GitHub Actions CI (Python 3.13)
