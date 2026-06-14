# CLAUDE.md — Le Carnet de Saison

Notice de projet pour Claude Code. À lire avant toute modification.

## Le projet en bref

Site web personnel de recettes, **100 % statique**, hébergé sur **GitHub Pages**.
- Dépôt : `user15051989/mes-recettes`
- URL en ligne : https://user15051989.github.io/mes-recettes/
- **Aucune étape de build, aucun framework, aucun serveur.** GitHub Pages republie
  automatiquement à chaque commit sur la branche `main`.

## Structure des fichiers

- `index.html` — **tout le site est ici** : le HTML, le CSS (dans `<style>`) et le
  JavaScript (dans `<script>`) sont dans ce seul fichier.
- `manifest.json` — rend l'app installable sur mobile.
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` — icônes de l'app.
- `og.png` — image d'aperçu lors d'un partage de lien.
- `img/` — photos des recettes (une par recette, optionnelles).

## Comment sont rangées les recettes

Toutes les recettes sont dans un tableau JavaScript nommé `recipes`, au milieu de
`index.html`. Chaque recette est un objet de cette forme :

```js
{
  cat:'saison',                 // catégorie : 'saison' | 'preg' | 'monde'
  tag:'De saison',              // libellé affiché sur la pastille
  motif:'leaf',                 // illustration de repli : leaf|sun|tomato|fish|wheat|bowl|egg
  color:'linear-gradient(...)', // fond coloré de la carte
  title:'Titre de la recette',
  desc:'Phrase d’accroche courte.',
  time:'25 min', serves:'2 pers.', diff:'Facile',
  ingredients:[ ['200 g','lentilles'], ['1','citron'] ],   // [quantité, nom]
  steps:[ 'Étape 1.', 'Étape 2.' ],
  badges:['Riche en fer'],      // OPTIONNEL — surtout pour la catégorie 'preg'
  why:'Pourquoi c’est recommandé…', // OPTIONNEL — encart, surtout 'preg'
  tip:'Astuce ou note de sécurité.'
}
```

Les onglets (Toutes / De saison / Femme enceinte / Du monde) filtrent par `cat`.
Les compteurs d'onglets et la recherche se mettent à jour automatiquement.

### Pour AJOUTER une recette
Copier un objet existant **dans le bon bloc de catégorie**, remplir les champs,
et veiller à **échapper les apostrophes** en JS : `l\'ail`, `d\'été`.
Garder le tableau `recipes` valide (virgules, guillemets) — c'est l'erreur la plus fréquente.

## Photos des recettes (convention importante)

Chaque recette cherche automatiquement une image : `img/{slug}.jpg`.
Le `slug` est dérivé du titre par cette règle (déjà codée dans `index.html`, fonction `slug()`) :
minuscules, accents retirés, tout caractère non `a-z0-9` remplacé par `-`.
Exemple : « Salade caprese tomates & mozzarella » → `salade-caprese-tomates-mozzarella.jpg`.

Si l'image n'existe pas, **l'illustration colorée s'affiche à la place** (pas de case vide).
Donc : ajouter une photo est toujours optionnel, mais le nom de fichier doit correspondre **exactement**.

## Règles éditoriales et de sécurité (NE PAS IGNORER)

1. **Authenticité, rien d'inventé.** Les recettes doivent être réelles et fidèles.
   Ne jamais inventer un plat, une quantité ou un fait. S'appuyer sur de vraies sources.
2. **Originalité.** Ne jamais copier-coller ni traduire mot pour mot le texte d'un blog,
   d'un livre ou d'une vidéo. Réécrire dans nos propres mots (respect du droit d'auteur).
3. **Section « Femme enceinte » (`cat:'preg'`)** — uniquement des plats sûrs, avec notes
   de sécurité exactes, en suivant les recommandations **suisses**, plus prudentes :
   - protéines (viande, volaille, poisson, œufs) **bien cuites à cœur** ;
   - fromages à pâte molle (mozzarella, féta…) seulement **bien cuits** dans un plat chaud ;
   - laver soigneusement les crudités ; produits laitiers **pasteurisés**.
   - Être **honnête** : si un plat n'est pas adapté dans sa version classique
     (ex. carbonara, œuf à peine cuit), le dire au lieu de prétendre le contraire.
4. **Pas un avis médical.** Conserver l'esprit du bandeau qui invite à confirmer
   les aliments adaptés avec une sage-femme ou un médecin.

## Conventions techniques

- **Design** : les couleurs et polices sont des variables CSS dans `:root`
  (vert « jardin » pour la saison, abricot pour la grossesse, fond papier ;
  polices Fraunces + DM Sans). Rester sobre et cohérent avec l'existant.
- **Aucune dépendance externe** sauf Google Fonts.
- **Liste de courses** : le regroupement des quantités ne fusionne que si le nom ET l'unité
  sont identiques et numériques. Ne pas forcer une fusion qui inventerait un total.
- **Stockage** : la liste de courses utilise `localStorage` enveloppé dans un `try/catch`
  (elle persiste sur le site en ligne ; ne pas retirer le `try/catch`).

## Vérifications avant de valider un changement

Le site étant en HTML/JS simple, ouvrir `index.html` et contrôler que :
- les 4 onglets filtrent correctement ;
- la barre de recherche fonctionne (par nom et par ingrédient) ;
- une fiche recette s'ouvre, le bouton « Ajouter à ma liste » et le panier marchent ;
- le tableau `recipes` reste du JavaScript valide.

## Déploiement

Aucune commande spéciale : tout commit sur `main` met le site à jour en 1–2 minutes
via GitHub Pages.
