# Yam's — Feuille de score

Feuille de score pour le Yam's (Yahtzee), pensée pour le mobile.
Un seul fichier `index.html`, aucune dépendance, aucun serveur : tout tourne dans le navigateur.

- 2 à 6 joueurs, noms modifiables avec rappel des joueurs habituels
- Totaux, bonus des 63 et bonus Yam's calculés automatiquement
- Rappel permanent de la règle sous chaque nom de combinaison, et règle détaillée avec
  exemple illustré en touchant le **nom**
- **Assistant de jeu** : quels dés garder, quelle case viser, espérance de points
- **Historique et statistiques** : victoires, taux de réussite, moyennes, records
- **Partie en ligne** : chacun sur son téléphone, scores en direct, on rejoint en scannant un QR code
- Sauvegarde automatique sur l'appareil (la partie survit à la fermeture de l'onglet)
- Thème clair / sombre selon les réglages du téléphone
- Compatible Safari iOS et Chrome Android

## Jouer en ligne, chacun sur son téléphone

Deux façons de jouer :

- **En local** (par défaut) : un seul téléphone tient la feuille de toute la table. Rien
  n'est envoyé sur Internet, aucun compte, fonctionne hors connexion.
- **En ligne** : chaque joueur a sa feuille sur son propre téléphone et voit les scores
  des autres se remplir en direct.

Pour lancer une partie en ligne : **Joueurs** (icône silhouettes) → **Créer une partie en
ligne**. Un QR code s'affiche ; les autres le scannent avec l'appareil photo de leur
téléphone et sont amenés directement sur la partie. Ils saisissent leur nom et jouent —
**aucun compte n'est nécessaire**.

- Chacun ne modifie que **sa** colonne ; l'organisateur peut corriger n'importe qui.
- L'organisateur peut ajouter un **joueur sans téléphone** (un enfant par exemple) et tenir
  sa colonne.
- Un bandeau sous la feuille indique l'état de la connexion, le code de la partie et le
  nombre de joueurs. Le bouton **QR** le ré-affiche à tout moment.
- La partie survit à une coupure réseau ou à la fermeture de l'onglet : à la réouverture,
  le téléphone se reconnecte tout seul.
- Quand tous ont terminé, chacun peut **Enregistrer** la partie dans son propre historique
  (avec les scores de tout le monde). L'organisateur qui **Quitte** ferme la partie pour tous.

### Ce que ça implique

Le jeu en ligne s'appuie sur **Firebase** (Google) : prénoms et scores de la partie
transitent par ce service, en Belgique, et sont effacés au bout de 30 jours. Le **mode local
reste inchangé** — hors ligne, sans compte, rien qui sorte de l'appareil.

La configuration Firebase (gratuite, déjà réalisée) est décrite dans
[SETUP-FIREBASE.md](SETUP-FIREBASE.md). Le site reste un fichier `index.html` unique hébergé
sur GitHub Pages : Firebase est seulement appelé depuis le navigateur, aucune bibliothèque
externe n'est chargée. Le QR code est généré par un encodeur maison (versions 1-5, niveau L),
validé par un décodeur indépendant.

> Étape indispensable une fois le site en ligne : autoriser le domaine `fofuls50.github.io`
> dans **Firebase → Authentication → Settings → Domaines autorisés**, sinon la connexion
> échouera sur le site publié.

## L'assistant de jeu

Bouton dé (vert) en haut à droite. On choisit le joueur, le jet en cours, puis on saisit
les 5 dés en touchant les faces. L'assistant répond par exemple :

> **Gardez ⚀⚁⚂⚃** — relancez 1 dé · en moyenne 33,1 pts sur ce tour
>
> *Ce que vous cherchez à décrocher*
> Petite suite ▓▓▓▓▓▓▓░ 83 % · 30 pts
> Grande suite ▓░░░░░░░ 17 % · 40 pts

La liste des objectifs est le point important : elle dit **pourquoi** on garde ces dés.
Les gardes alternatives affichent elles aussi leurs objectifs et leurs probabilités,
ce qui permet de comparer les stratégies plutôt que de suivre le conseil à l'aveugle.
Quand la garde conseillée rapporte un peu moins que la meilleure du tour, l'assistant
nomme l'alternative et explique l'arbitrage (généralement le bonus des 63).

Au 3ᵉ jet il indique la meilleure case et propose de l'inscrire directement.

Le calcul est exact sur le tour en cours : il énumère les 462 façons de garder des dés,
les 252 tirages possibles, et remonte l'espérance sur les relances restantes (~10 ms).
Le conseil tient compte des cases déjà remplies, du bonus des 63 encore atteignable et du
bonus Yam's à +100. Chaque case est évaluée en *gain par rapport à sa valeur normale*,
pour éviter de brûler la Chance ou le Brelan sur un tirage moyen.

C'est une heuristique très forte, pas le solveur optimal complet du Yahtzee (qui exigerait
une table précalculée de plusieurs centaines de milliers d'états) : sur un tour donné
l'écart avec le jeu parfait est de l'ordre de quelques points sur une partie.

## Publier gratuitement sur GitHub Pages

1. Créer un dépôt **public** sur GitHub, par exemple `yamtzee`.
2. Y déposer `index.html` (bouton *Add file → Upload files*, ou en ligne de commande ci-dessous).
3. Aller dans **Settings → Pages**.
4. Sous *Build and deployment* → *Source*, choisir **Deploy from a branch**, puis la branche `main` et le dossier `/ (root)`. Cliquer sur **Save**.
5. Attendre ~1 minute : le site est en ligne sur `https://<ton-pseudo>.github.io/yamtzee/`

En ligne de commande, depuis ce dossier :

```bash
git init && git add . && git commit -m "Feuille de score Yam's" && git branch -M main && git remote add origin https://github.com/<ton-pseudo>/yamtzee.git && git push -u origin main
```

## Joueurs habituels

Dans la fenêtre **Joueurs**, toucher un champ de nom fait apparaître les joueurs déjà
enregistrés dans l'historique, les plus fréquents d'abord : un appui suffit pour reprendre
le nom. Les noms déjà attribués aux autres joueurs de la partie ne sont pas proposés.

C'est aussi ce qui garantit des statistiques justes : une orthographe identique d'une partie
à l'autre regroupe les résultats sur le même joueur. Les champs de la feuille de score
proposent les mêmes noms en saisie automatique.

## Historique et statistiques

Quand les 13 cases de tous les joueurs sont remplies, une bannière annonce le vainqueur et
propose **Enregistrer**. La partie rejoint alors l'historique (bouton graphique en haut à gauche) :

- parties jouées, record absolu, moyenne générale
- par joueur : nombre de parties, victoires et taux, score moyen, record
- taux de réussite du bonus des 63 et nombre de Yam's réalisés
- les 12 dernières parties avec date, vainqueur et scores

Les égalités comptent comme une victoire pour chaque joueur concerné.

### Sauvegarde et transfert entre appareils

Trois mécanismes se complètent :

1. **Stockage durable** — option désactivée par défaut, à activer dans les **Réglages**
   (roue dentée). Elle demande au navigateur de ne pas effacer les données lors d'un
   nettoyage automatique (`navigator.storage.persist`). Rien n'est demandé tant que
   l'interrupteur n'est pas actionné : la fenêtre d'autorisation du navigateur n'apparaît
   qu'après avoir lu l'explication et touché le bouton.

   L'écran affiche l'état réel : accordé, refusé, demandé mais pas encore accordé (fréquent
   tant que le site n'est pas sur l'écran d'accueil), désactivé, ou non géré par le navigateur.
   Une autorisation déjà accordée ne peut pas être retirée par une page web — il faut
   effacer les données du site dans les réglages du navigateur, ce que l'écran indique.

   > Détail d'implémentation important : la réponse de `persist()` est mémorisée et fait
   > autorité sur `persisted()`. Firefox peut en effet répondre « oui » à `persisted()`
   > alors que l'utilisateur vient de bloquer la demande sans cocher « se souvenir de cette
   > décision » — se fier à `persisted()` seul affichait alors un « Actif » mensonger.
2. **Exporter / Importer** — un fichier JSON. Sur téléphone, l'export passe par le partage
   natif : on envoie sa sauvegarde par mail, message ou AirDrop en deux gestes. Sur
   ordinateur, c'est un téléchargement classique. L'import fusionne sans créer de doublon
   (les parties sont identifiées par leur date), ce qui permet aussi de réunir l'historique
   de deux téléphones.
3. **Rappel** — au-delà de 8 parties sans sauvegarde, un bandeau le signale dans les
   statistiques.

> À savoir sur iPhone : Safari peut effacer les données d'un site resté inutilisé plusieurs
> semaines. Les points 1 et 2 couvrent ce risque.

## Ajouter à l'écran d'accueil

- **iPhone (Safari)** : bouton Partager → *Sur l'écran d'accueil*
- **Android (Chrome)** : menu ⋮ → *Ajouter à l'écran d'accueil*

L'application s'ouvre alors en plein écran, sans barre d'adresse.

## Règles appliquées

| Case | Score |
|---|---|
| As → Six | nombre de dés × la valeur |
| Bonus section haute | +35 si le total haut ≥ 63 |
| Brelan, Carré, Chance | somme des 5 dés |
| Full | 25 |
| Petite suite | 30 |
| Grande suite | 40 |
| Yam's | 50 |
| Yam's supplémentaire | +100 chacun |

Une case barrée (✕) vaut 0.
