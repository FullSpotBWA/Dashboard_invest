# Vue d'ensemble du projet

## Qu'est-ce que c'est ?

**Dashboard Invest** est un tableau de bord personnel de suivi de portefeuille boursier, écrit en HTML/CSS/JavaScript vanilla et déployé sur GitHub Pages.

L'objectif est d'avoir une interface claire et légère pour suivre ses positions d'investissement sans dépendre d'un service tiers payant ou d'une application mobile.

## Profil utilisateur

- Investisseur particulier débutant à intermédiaire
- Horizon d'investissement long terme
- Stratégie d'entrée progressive (DCA)
- Plateformes utilisées : **Revolut** et **Trade Republic**

## Portefeuille suivi

| Ticker | Nom | Catégorie | Plateforme |
|--------|-----|-----------|------------|
| BTC | Bitcoin | Crypto | Revolut + Trade Republic |
| SOL | Solana | Crypto | Revolut |
| PLTR | Palantir | Action | Revolut |
| GOOGL | Alphabet | Action | Revolut |
| SPX | S&P 500 ETF | ETF | Trade Republic |
| TBSO | TBSO | Action | Trade Republic |

## Source de données

Les prix actuels et les positions sont synchronisés depuis un **Google Sheets** publié en CSV.  
En cas d'indisponibilité, un jeu de données de démonstration statique est affiché en fallback.

## Déploiement

L'application est déployée automatiquement sur **GitHub Pages** à chaque push sur la branche `main` via GitHub Actions.

URL de déploiement : `https://fullspotbwa.github.io/Dashboard_invest/`
