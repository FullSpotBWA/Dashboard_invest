# Bugs connus & feuille de route

## Bugs corrigés

| # | Description | Statut |
|---|-------------|--------|
| B1 | **Chart.js cassé** — référence locale remplacée par CDN jsDelivr | ✅ Corrigé |
| B2 | **Google Sheets jamais chargé** — condition toujours vraie remplacée par check `!CSV_URL` | ✅ Corrigé |
| B2b | **Parsing CSV format français** — nombres `"$61 819,34"` mal parsés (espace = séparateur milliers, virgule = décimale). Ajout de `cleanNum()` | ✅ Corrigé |
| B2c | **Index colonne prix actuel erroné** — col. 10 = valeur totale, col. 9 = prix unitaire. `CSV_COLS.px` corrigé à 9 | ✅ Corrigé |
| B3 | **Positions perdues au refresh** — localStorage pour les positions ajoutées manuellement + badge LOCAL + bouton supprimer | ✅ Corrigé |
| B4 | **Historique perdu au refresh** — localStorage pour l'historique mensuel | ✅ Corrigé |
| B5 | **Saisie historique via `prompt()`** — remplacé par formulaire inline avec validation | ✅ Corrigé |

---

## Feuille de route

### Phase 1 — Données live watchlist

- [ ] Connecter la watchlist à une API de prix (CoinGecko pour les cryptos, Yahoo Finance / Alpha Vantage pour les actions/ETF)
- [ ] Ajouter un timestamp "dernière mise à jour" sur chaque carte watchlist

### Phase 2 — Améliorations UX

- [ ] Tri par colonne dans le tableau des positions (clic sur en-tête)
- [ ] Vue "par actif regroupé" — consolider les lignes BTC en une seule ligne agrégée
- [ ] Filtre par plateforme / catégorie dans l'onglet Positions

### Phase 3 — Nouvelles fonctionnalités

- [ ] Export CSV du portefeuille
- [ ] Graphique de répartition cible vs réelle (objectif d'allocation)
- [ ] Alerte de seuil (notifier si une position dépasse ±X%)
