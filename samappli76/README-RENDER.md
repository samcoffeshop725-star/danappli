# Publier la boutique sur Render — 5 minutes

Ce dossier contient **toute l'application dans 2 fichiers** : `server.js` (serveur + pages + données) et `package.json`.
Il n'y a rien à installer, aucun dossier `public` à envoyer.

## 1. GitHub
Dans votre dépôt : **Add file → Upload files**, glissez **`server.js`** et **`package.json`**, puis **Commit changes**.
⚠️ Les deux fichiers doivent être à la racine du dépôt (pas dans un sous-dossier).
Si d'anciens fichiers cassés s'y trouvent déjà, supprimez-les avant.

## 2. Render
**New → Web Service** → choisissez le dépôt, puis :

| Réglage | Valeur |
|---|---|
| Language / Runtime | **`Node`** ← le plus important |
| Build Command | `echo ok` (ou laisser vide) |
| Start Command | `node server.js` |

### ❌ « Build failed » avec `mix phx.digest` / `no mix.exs was found`
Render a choisi le langage **Elixir** au lieu de Node. Deux solutions :
1. **Settings** du service → champ **Language** → `Node` → *Build Command* : `echo ok` → *Start Command* : `node server.js` → **Save**, puis **Manual Deploy → Clear build cache & deploy**.
2. Si le champ Language ne se modifie pas : supprimez le service (*Settings → Delete Web Service*) et recréez-le avec **New → Web Service**, en choisissant **Node**.

Le fichier `render.yaml` fourni ici décrit déjà ces réglages : si vous créez le service via **New → Blueprint**, tout est configuré automatiquement.

## 3. Variables (onglet *Environment*)

| Clé | Valeur |
|---|---|
| `TZ` | `Europe/Paris` |
| `WEBAPP_URL` | l'adresse du service, ex. `https://sam751.onrender.com` |
| `BOT_TOKEN` | le token donné par @BotFather |

Vos identifiants admin (numéro + mot de passe) sont **déjà intégrés** dans `server.js`, il n'y a rien à ajouter pour se connecter.
Le mot de passe est stocké **haché** (scrypt) : il n'apparaît en clair nulle part dans le fichier.

### Changer le mot de passe
Panel admin → onglet **Général** → **🔒 Mes identifiants de connexion**. Le changement est immédiat et déconnecte les autres sessions.
⚠️ Sur l'offre gratuite Render (sans disque persistant), ce changement est perdu au redémarrage du service : ajoutez alors
`ADMIN_PHONE` et `ADMIN_PASSWORD` dans *Environment* — ces variables reprennent la main tant qu'aucun changement n'a été fait depuis le panel.

## 4. Vérifier

**Vérifier la version installée** : panel admin → onglet **Intégrations**, première ligne. La version actuelle est `1.4 — image + vidéo par produit`.
Elle est aussi visible dans `https://votre-adresse/api/shop` (champ `version`).

- `https://votre-adresse/` → la boutique s'affiche.
- `https://votre-adresse/admin` → la page « Accès admin ».
- `https://votre-adresse/api/shop` → du texte JSON avec vos produits.

## 5. Telegram
@BotFather → `/newbot` → copiez le token dans `BOT_TOKEN`, mettez votre adresse Render dans `WEBAPP_URL`, redéployez.
Envoyez ensuite `/start` à votre bot : le bouton **🛍️ Ouvrir la boutique** apparaît.

## Vidéos des produits
Panel admin → **Produits** → *Modifier*. Un produit peut avoir **une image ET une vidéo** : remplissez les deux champs, ils sont indépendants.
Vidéo : importez un MP4, WEBM ou MOV (80 Mo max) ou collez un lien YouTube, Vimeo ou MP4.
L'**image et la vidéo s'affichent toutes les deux** dans la fiche produit : l'image en haut, la vidéo juste en dessous. Une pastille ▶ marque le produit dans la liste.
Sur un hébergement sans disque persistant (Render gratuit), préférez un **lien** : les fichiers importés sont effacés à chaque redémarrage.

## À savoir (offre gratuite Render)
- Le service s'endort après 15 min sans visite ; le premier chargement prend alors ~30 s.
- Le disque n'est pas conservé : à chaque redéploiement ou réveil, la boutique repart du contenu intégré dans `server.js`
  (vos 6 produits SAM COFFEE SHOP). Les produits, images et commandes ajoutés depuis l'admin en ligne sont perdus.
  Pour garder vos données : disque persistant (offre payante), VPS, ou demandez-moi une version qui les stocke ailleurs.
