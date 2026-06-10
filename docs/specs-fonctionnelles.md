# Spécifications fonctionnelles

## 1. KPI globaux (bandeau en haut)

Affiche en permanence les métriques clés du portefeuille, recalculées à chaque rafraîchissement.

| KPI | Description |
|-----|-------------|
| Valeur totale | Somme de la valeur actuelle de toutes les positions |
| Capital investi | Somme des montants investis |
| Plus-value | Différence valeur – investi, affichée avec % |
| Meilleure position | Actif avec le plus fort % de gain |
| À surveiller | Actif avec le plus fort % de perte |

## 2. Onglets

### 2.1 Positions

Tableau de toutes les lignes du portefeuille.

| Colonne | Contenu |
|---------|---------|
| Actif | Ticker + badge catégorie + badge plateforme + nom + date d'achat |
| Investi | Montant investi (converti dans la devise choisie) |
| Valeur actuelle | Valeur calculée : `(investi / prix_achat) × prix_actuel` |
| +/- | Plus ou moins-value absolue |
| +/- % | Plus ou moins-value en pourcentage |

### 2.2 Répartition

Trois graphiques de répartition du portefeuille :

- **Par actif** — donut avec légende (BTC, SOL, SPX, etc.)
- **Par catégorie** — donut (Crypto / Action / ETF)
- **Par plateforme** — barres horizontales comparant investi vs valeur (Revolut / Trade Republic)

### 2.3 Progression

- Graphique linéaire de l'évolution mensuelle (valeur + investi)
- Tableau des points historiques avec : date, valeur, investi, P&L, variation vs mois précédent
- Bouton pour ajouter manuellement un point mensuel

### 2.4 Performance

- Graphique en barres groupées : investi vs valeur actuelle, par actif
- Graphique en barres horizontales : plus-value par position individuelle (vert = gain, rouge = perte)

### 2.5 Watchlist

Liste d'actifs suivis mais pas encore en portefeuille. Pour chacun :

- Ticker, nom, secteur
- Prix indicatif
- Variations : 1 jour / 1 semaine / 1 mois / YTD
- Signal **Consensus analystes** (marché)
- Signal **Ma reco** (adapté au profil investisseur)
- Note d'analyse détaillée

## 3. Fonctionnalités transversales

### Sélecteur de devise
Bascule entre **€ EUR** et **$ USD** via un taux de change EUR/USD modifiable manuellement.

### Auto-refresh
Rechargement automatique des données Google Sheets toutes les **5 minutes** avec :
- Barre de progression visuelle
- Compte à rebours
- Bouton Pause / Reprendre

### Ajout de position (modal)
Formulaire pour ajouter une nouvelle position avec les champs :
- Ticker, Nom complet, Catégorie, Plateforme
- Date d'achat (format JJ/MM/AAAA)
- Montant investi, Prix d'achat, Prix actuel

### Mode sombre
Interface adaptée automatiquement selon la préférence système (`prefers-color-scheme: dark`).

### Responsive
Colonnes masquées sur mobile (< 650px) pour conserver la lisibilité.

## 4. Format CSV Google Sheets attendu

Le CSV publié depuis Google Sheets doit respecter l'ordre de colonnes suivant :

| Index | Colonne |
|-------|---------|
| 0 | (ignoré) |
| 1 | Type (Crypto / Action / ETF) |
| 2 | Nom complet |
| 3 | Plateforme |
| 4 | Date d'achat |
| 6 | Prix d'achat ($) |
| 7 | Montant investi ($) |
| 10 | Prix actuel ($) |

Les lignes contenant "total" ou vides sont ignorées. La première ligne (en-tête) est skippée.
