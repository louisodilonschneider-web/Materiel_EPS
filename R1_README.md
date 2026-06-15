# EPS Inventaire — Documentation

Application web autonome (fichier HTML unique) de gestion du matériel pour enseignants d'EPS.

---

## 🚀 Démarrage

Ouvrir **`eps-inventaire.html`** dans un navigateur moderne (Chrome, Firefox, Edge).  
Aucune installation, aucun serveur requis. Les données sont sauvegardées dans le `localStorage` du navigateur.

---

## 📄 Pages

### 1. Récapitulatif
- Cartes de synthèse : nb d'articles, unités totales, états
- **Barre de progression globale** : % Correct / Dégradé / Inutilisable
- **Détail par article** avec barre d'état individuelle et pourcentages
- Bouton modifier ✏️ et supprimer 🗑 pour chaque article

### 2. Ajouter matériel
- Champs : **nom** (obligatoire), quantité, date d'entrée, référence Casal (optionnel), sport/activité, notes
- **État général** (identique pour toutes les unités) OU **état par unité** (U1, U2, U3…)
- Recherche dans le catalogue Casal pour pré-remplir la référence
- **Upload de photos** (stockées en base64 dans le navigateur)
- États : ✅ Correct / ⚠️ Dégradé / ❌ Inutilisable

### 3. Rechercher
- **Onglet Catalogue Casal** : recherche par mot-clé, sport, catégorie — boutons pour ajouter à l'inventaire ou à la commande
- **Onglet Mon inventaire** : recherche dans les articles déjà enregistrés

### 4. Commander
- **Auto-peuplé** avec le matériel inutilisable de l'inventaire
- Ajout manuel depuis le catalogue Casal ou en article libre
- Ajustement de la quantité par article (boutons +/−)
- **Prix indicatif** affiché si l'article est référencé Casal
- **Total estimatif** de la commande
- Bouton "Passer la commande" → bascule en historique

### 5. Historique
- Toutes les commandes passées avec statut **En cours / Arrivée**
- Bouton **"Marquer arrivée"** : met à jour l'inventaire automatiquement (les unités Inutilisable passent à Correct)
- Journalisation des ajouts et réparations

### 6. Réparation
- Liste du matériel **Dégradé** et/ou **Inutilisable**
- **Tri** : par priorité (inutilisable d'abord), par nom, par quantité
- **Filtres** : tous / inutilisable seulement / dégradé seulement
- Saisie du nombre d'unités réparées → mise à jour immédiate de l'inventaire
- Événement journalisé dans l'historique

---

## 🗂 Catalogue Casal Sport intégré

34 articles couvrant :
- Athlétisme (plots, haies, lancer, mesure…)
- Jeux collectifs (ballons, chasubles, buts, filets)
- Gymnique (tapis, agrès, cordes)
- Raquettes (badminton, tennis, tennis de table)
- Natation, Outdoor, Accessoires généraux

Chaque article contient : `ref`, `nom`, `sport`, `cat`, `marque`, `prix`, `desc`

### Ajouter vos propres articles Casal
Dans le fichier HTML, repérer le tableau `const CASAL = [...]` et ajouter des entrées au format :
```js
{ ref:"REF-001", nom:"Nom du produit", sport:"Sport", cat:"Catégorie", marque:"Marque", prix:0.00, desc:"Description" }
```

Si vous disposez du fichier `casal-catalogue-import.json`, remplacer le tableau `CASAL` par :
```js
const CASAL = /* contenu du JSON parsé */ ;
```

---

## 💾 Stockage des données

- **localStorage** du navigateur (clé : `eps_db`)
- Structure : `{ inventaire: [...], commande: [...], historique: [...] }`
- Les images sont encodées en base64 → attention à la taille pour les grandes photos
- Pour **sauvegarder** : exporter le contenu de `localStorage` via la console :
  ```js
  copy(localStorage.getItem('eps_db'))
  ```
- Pour **restaurer** :
  ```js
  localStorage.setItem('eps_db', '<json copié>')
  ```

---

## 🔄 Flux de travail recommandé

```
Ajout matériel
    ↓
Récapitulatif (suivi état)
    ↓
Matériel inutilisable → auto-ajouté à Commander
    ↓
Commander → Passer la commande
    ↓
Historique → Marquer arrivée → inventaire mis à jour
```
```
Matériel dégradé/inutilisable → Réparation
    ↓
Saisir nb d'unités réparées → inventaire mis à jour
```

---

## 🛠 Évolutions envisagées

- [ ] Export CSV / PDF de l'inventaire
- [ ] Import du JSON Casal complet
- [ ] Export de la commande en PDF / email
- [ ] Multi-établissements
- [ ] Synchronisation cloud (Firebase, Supabase)
- [ ] Code QR par article pour inventaire rapide

---

*Version 1.0 — Juin 2025*
