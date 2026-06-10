# Architecture technique

## Stack

| Couche | Technologie |
|--------|-------------|
| Frontend | HTML5 / CSS3 / JavaScript vanilla (ES2020) |
| Graphiques | Chart.js (UMD) |
| Données | Google Sheets → CSV public |
| Hébergement | GitHub Pages |
| CI/CD | GitHub Actions |

## Structure des fichiers

```
Dashboard_invest/
├── index.html                    # Application complète (HTML + CSS + JS)
├── docs/                         # Documentation du projet
│   ├── README.md
│   ├── project-overview.md
│   ├── specs-fonctionnelles.md
│   ├── architecture.md
│   └── roadmap.md
└── .github/
    └── workflows/
        └── static.yml            # Déploiement GitHub Pages
```

## Organisation du code (index.html)

Le fichier `index.html` est structuré en trois blocs :

### 1. CSS (lignes ~8–101)
Entièrement inline, organisé par composant :
- Variables CSS (thème clair + dark mode)
- Layout global (`.wrap`, `.top`)
- Composants UI (`.kpi`, `.card`, `.tab`, `.modal`, `.toast`, etc.)
- Responsive media query

### 2. HTML (lignes ~103–337)
Structure statique initiale, remplacée dynamiquement par JavaScript :
- Bandeau KPI (`#kpis`)
- Onglets (`#vpos`, `#valloc`, `#vprog`, `#vperf`, `#vwatch`)
- Modal d'ajout de position (`#overlay`)
- Barre auto-refresh

### 3. JavaScript (lignes ~338–686)
Organisé par responsabilité :

```
CONFIG
  └── CSV_URL                 URL Google Sheets

DONNÉES STATIQUES
  ├── FALLBACK_POS            14 positions (fallback + état initial)
  └── WATCH                   5 actifs watchlist

ÉTAT
  ├── pos[]                   Positions actives (chargées depuis CSV ou fallback)
  ├── hist[]                  Points historiques mensuels
  ├── cur                     Devise active ('EUR' | 'USD')
  └── ch{}                    Instances Chart.js en mémoire

UTILITAIRES
  ├── getFx()                 Lecture du taux EUR/USD
  ├── cv(usd)                 Conversion USD → devise active
  ├── f2(usd)                 Format monétaire (2 décimales)
  ├── f0(usd)                 Format monétaire (0 décimale)
  ├── fp(v)                   Format pourcentage
  └── calc(p)                 Calcule {n, val, pl, pct} pour une position

CHARGEMENT
  ├── loadCSV()               Fetch + parse Google Sheets CSV
  └── setStatus(type, msg)    Met à jour la barre de statut

RENDER
  ├── rKPI()                  Bandeau KPI
  ├── rPos()                  Tableau des positions
  ├── rAlloc()                Graphiques de répartition
  ├── rProg()                 Graphique + tableau de progression
  ├── rPerf()                 Graphiques de performance
  ├── rWatch()                Watchlist
  └── rAll()                  Appelle tous les renders actifs

NAVIGATION
  └── sv(v)                   Affiche l'onglet demandé

AUTO-REFRESH
  ├── startAutoRefresh()
  └── toggleAutoRefresh()

MODAL
  ├── openModal() / closeModal()
  └── submitPos()             Validation + ajout en mémoire
```

## Flux de données

```
Démarrage (DOMContentLoaded)
        │
        ▼
   loadCSV()
        │
        ├── Succès → pos = données CSV → rAll()
        └── Échec  → pos = FALLBACK_POS → rAll()
                                │
                    ┌───────────┴───────────┐
                    ▼                       ▼
              rKPI() rPos()         rAlloc/rProg/rPerf/rWatch
                                   (seulement si onglet visible)
```

## Calcul d'une position

```
n   = inv / pa          (nombre d'unités)
val = n × px            (valeur actuelle)
pl  = val - inv         (plus-value absolue)
pct = pl / inv × 100    (plus-value en %)
```

Toutes les valeurs sont stockées en **USD** et converties à l'affichage selon la devise choisie.
