# Guide d'utilisation

## Comment fonctionne le dashboard

Le dashboard est une page web statique (`index.html`) hébergée sur **GitHub Pages**.  
Il n'y a pas de serveur, pas de base de données — tout tourne dans le navigateur.

Les données viennent de deux sources :

| Source | Rôle |
|--------|------|
| **Google Sheets** | Source de vérité — prix actuels automatiques via Google Finance |
| **localStorage** | Sauvegarde locale des positions ajoutées manuellement |

---

## Comment accéder au dashboard

| Méthode | URL | Synchro Google Sheets |
|---------|-----|-----------------------|
| ✅ GitHub Pages | `https://fullspotbwa.github.io/Dashboard_invest/` | ✅ Fonctionne |
| ✅ Serveur local | `http://localhost:3000` | ✅ Fonctionne |
| ❌ Fichier local | Double-clic sur `index.html` | ❌ Bloqué par le navigateur |

> **Pourquoi le fichier local ne fonctionne pas ?**  
> Les navigateurs bloquent les requêtes réseau depuis le protocole `file://` pour des raisons de sécurité. C'est normal. Utilise toujours GitHub Pages pour consulter ton portefeuille.

---

## Comment fonctionne l'actualisation des prix

Les prix sont mis à jour **automatiquement par Google Sheets** via la formule `GOOGLEFINANCE`.  
Le dashboard n'appelle aucune API directement — il lit simplement le CSV publié par le sheet.

```
Google Finance → Google Sheets (mise à jour automatique)
                      ↓
              CSV publié en public
                      ↓
           Dashboard (lecture au chargement)
```

### Quand les prix se mettent-ils à jour ?

- **Google Sheets** rafraîchit les prix via `GOOGLEFINANCE` toutes les **~20 minutes** en journée boursière.
- **Le dashboard** relit le CSV à chaque fois que tu cliques sur **⟳ Actualiser** ou que tu recharges la page.

### Comment rafraîchir manuellement

1. Ouvrir le dashboard sur GitHub Pages
2. Cliquer sur le bouton **⟳ Actualiser** en haut à droite
3. La barre de statut passe à `✅ Google Sheets synchronisé` et affiche l'heure

---

## Les onglets

| Onglet | Ce qu'il montre |
|--------|-----------------|
| **Positions** | Toutes les lignes du portefeuille avec P&L en temps réel |
| **Répartition** | Graphiques de répartition par actif, catégorie, plateforme |
| **Progression** | Évolution mensuelle de la valeur du portefeuille |
| **Performance** | Comparaison investi vs valeur par actif + P&L par ligne |
| **Watchlist** | Actifs suivis avec signal analystes et recommandation perso |

---

## Ajouter une position manuellement

1. Cliquer sur **+ Position**
2. Remplir le formulaire (ticker, nom, catégorie, plateforme, date, montant, prix)
3. Cliquer sur **Ajouter ✓**

> La position est sauvegardée dans le **localStorage** du navigateur.  
> Elle persiste entre les rechargements mais reste locale à ton appareil et navigateur.  
> Elle apparaît avec le badge **LOCAL** dans la liste des positions.  
> Pour la supprimer, cliquer sur le **✕** à droite de la ligne.

> **Bonne pratique** : ajouter la position dans Google Sheets en parallèle pour qu'elle soit incluse dans la source de vérité.

---

## Le Google Sheet

Le sheet est structuré ainsi :

- **Colonne F** : ticker Google Finance (ex: `CURRENCY:BTCUSD`, `NASDAQ:PLTR`)
- **Colonne G** : prix d'achat (saisi manuellement en $)
- **Colonne H** : montant investi (saisi manuellement en $)
- **Colonne J** : prix actuel (calculé automatiquement via `GOOGLEFINANCE`)

Les colonnes calculées (valeur actuelle, P&L, etc.) sont recalculées par Sheets et lues par le dashboard.

Pour ajouter une position dans Sheets : dupliquer une ligne existante et adapter les colonnes G, H, et le ticker en F.
