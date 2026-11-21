# Anime Requester 🎬

[![CI Lint and Format](https://github.com/Clemzzz/anime-requester/actions/workflows/ci.yml/badge.svg)](https://github.com/Clemzzz/anime-requester/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Anime Requester** est une application web moderne et responsive permettant de rechercher et d'explorer une vaste base de données d'animes via l'API **Tenrai API v1** (`https://api.tenrai.org/v1`, compatible Jikan v4 / MyAnimeList).

Développée dans le cadre d'un projet de groupe en JavaScript (BUT Informatique), l'application met l'accent sur la manipulation du DOM en modules ES6 natifs, les requêtes HTTP asynchrones, l'accessibilité WCAG AA et l'ingénierie logicielle (tests unitaires Node.js + JSDOM, linting ESLint, formatage Prettier, hooks Husky et CI GitHub Actions).

---

## 🌟 Fonctionnalités

- **Recherche multicritère :**
  - Par **titre**
  - Par **identifiant** unique (`MAL ID`)
  - Par **classement** (`Ranking`)
  - Par **genre** (en français, en anglais ou par identifiant)
  - Par **auteur** (résolution d'auteur puis récupération de ses œuvres)
  - Par **année de sortie**
- **Affichage conditionnel :**
  - **Recherche classique :** limitée à **10 résultats maximum** pour un affichage lisible et performant.
  - **Recherche par ID ou classement :** affichage d'un **résultat unique et ciblé**.
- **Cartes d'anime détaillées :**
  - Titre de l'œuvre et affiche / image de couverture (avec fallback SVG accessible si indisponible)
  - Synopsis complet, genres associés, classement officiel et nombre d'épisodes
- **Gestion optionnelle de la clé API :**
  - Fonctionnement immédiat sans clé obligatoire grâce à l'API publique Tenrai v1.
  - Saisie et stockage temporaire d'une clé personnalisée en `sessionStorage`, avec bouton pour afficher/masquer la valeur.
- **Réinitialisation en un clic :**
  - Bouton **Effacer** réinitialisant les filtres du formulaire et vidant les résultats affichés tout en préservant la clé stockée en session.
- **Accessibilité & Design :**
  - Structure HTML5 sémantique conforme **WCAG AA 2.0** (`aria-live`, `role="alert"`, `aria-describedby`, textes alternatifs).
  - Interface responsive pilotée par des variables CSS (`css/variables.css`).

---

## 🛠 Stack Technique

- **Langages :** HTML5 sémantique, CSS3 (Custom Properties, Flexbox & Grid), JavaScript (ES6+ Modules natifs)
- **Client HTTP :** [Axios](https://axios-http.com/) (chargé via ESM `importmap` côté navigateur et package npm côté tests)
- **API distante :** [Tenrai API v1](https://api.tenrai.org/v1)
- **Qualité de code :**
  - [ESLint](https://eslint.org/) (Flat Config)
  - [Prettier](https://prettier.io/)
  - [Husky](https://typicode.github.io/husky/) & [lint-staged](https://github.com/lint-staged/lint-staged) pour les hooks pre-commit
- **Tests :** Test runner natif Node.js (`node:test`, `node:assert`) avec [JSDOM](https://github.com/jsdom/jsdom)
- **Intégration Continue (CI) :** GitHub Actions (validation Prettier, ESLint et tests unitaires à chaque push et PR sur `main` et `develop`)

---

## 📂 Architecture du Projet

```text
anime-requester/
├── .github/
│   ├── ISSUE_TEMPLATE/          # Modèles d'issues (bug, feature, tâche)
│   ├── workflows/
│   │   └── ci.yml               # Pipeline d'intégration continue GitHub Actions
│   └── pull_request_template.md # Template standardisé de Pull Request
├── .husky/                      # Configuration des hooks Git pre-commit
├── css/
│   ├── style.css                # Styles principaux et mise en page responsive
│   └── variables.css            # Variables globales (couleurs, typographie, espacements)
├── js/
│   ├── animeApi.js              # Requêtes vers Tenrai API v1 (Axios) et normalisation des données
│   ├── apiKeyStorage.js         # Stockage et lecture de la clé API en sessionStorage
│   ├── authorSearch.js          # Résolution d'auteur et récupération de ses animes
│   ├── cardRenderer.js          # Rendu DOM des cartes et logique d'affichage conditionnel
│   └── searchController.js      # Contrôleur d'événements du formulaire et liaison UI/API
├── tests/
│   ├── animeApi.test.js         # Tests unitaires du client API et de la normalisation
│   ├── apiKeyStorage.test.js    # Tests unitaires du stockage de la clé API
│   ├── authorSearch.test.js     # Tests unitaires de la recherche par auteur
│   ├── cardRenderer.test.js     # Tests unitaires du rendu DOM et de l'affichage
│   └── searchController.test.js # Tests unitaires du contrôleur et des événements
├── index.html                   # Page principale de l'application (importmap Axios)
├── eslint.config.js             # Configuration ESLint (Flat Config)
├── package.json                 # Scripts NPM et dépendances de développement
├── LICENSE                      # Licence MIT
└── README.md                    # Documentation du projet
```

### Flux applicatif

```text
Formulaire (index.html)
  → searchController.js (validation, gestion d'événements, sessionStorage via apiKeyStorage.js)
  → animeApi.js / authorSearch.js (requêtes HTTP Axios vers Tenrai API v1 + normalisation)
  → cardRenderer.js (rendu DOM accessible des cartes et des messages d'état)
```

---

## 🚀 Installation et Utilisation

### Prérequis

- [Node.js](https://nodejs.org/) (version 20 LTS ou supérieure recommandée)
- [npm](https://www.npmjs.com/)

### 1. Cloner le dépôt

```bash
git clone https://github.com/Clemzzz/anime-requester.git
cd anime-requester
```

### 2. Installer les dépendances

```bash
npm install
# ou (recommandé en CI)
npm ci
```

### 3. Lancer l'application en local

```bash
npm run dev
# ou
npx serve .
```

Puis ouvrez votre navigateur à l'adresse indiquée (par défaut : `http://localhost:3000`).

> **Note :** l'application utilise des modules ES natifs (`type="module"`) et un `importmap` ; elle doit donc être servie via un serveur HTTP local (`npm run dev`) et non ouverte directement en `file://`.

---

## 🧪 Tests et Qualité de Code

Le projet intègre un ensemble de scripts automatisés exécutés également par la CI :

| Commande                 | Description                                                             |
| ------------------------ | ----------------------------------------------------------------------- |
| `npm run dev`            | Démarre un serveur HTTP local via `npx serve .`                         |
| `npm test`               | Exécute la suite de tests unitaires via le test runner natif de Node.js |
| `npm run lint`           | Vérifie la conformité du code avec ESLint                               |
| `npm run format`         | Applique le formatage automatique avec Prettier                         |
| `npx prettier --check .` | Vérifie que l'ensemble des fichiers respecte le style Prettier          |

---

## 🔧 Dépannage

- **Erreur CORS ou module non chargé à l'ouverture de `index.html` :** lancez bien l'application via `npm run dev` (HTTP) plutôt qu'en ouvrant directement le fichier depuis l'explorateur (`file://`).
- **Erreur de formatage en CI (`prettier --check`) :** exécutez `npm run format` avant de commiter vos modifications.

---

## 👥 Auteurs & Licence

Projet développé en groupe (Clément Bazin [@Clemzzz](https://github.com/Clemzzz), Paul Maillard [@Paul272007](https://github.com/Paul272007) et l'équipe **Anime Requester**).

Distribué sous licence **MIT**. Voir le fichier [LICENSE](./LICENSE) pour plus d'informations.
