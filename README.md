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
- **Feuille sur un seul écran** : la grille s'ajuste pour être visible en entier, sans défilement
- **Écran maintenu allumé** pendant la partie (désactivable dans les réglages)
- Thème clair / sombre selon les réglages du téléphone
- Compatible Safari iOS et Chrome Android

## Lancer une partie

Depuis l'accueil, **Nouvelle partie** ouvre un assistant en trois écrans — une question
par écran, avec un retour possible à tout moment :

1. **Comment jouez-vous ?** *Sur cet appareil* (une seule feuille, aucun réseau) ou
   *Chacun son téléphone* (partie en ligne, créée immédiatement).
2. **Qui joue ?** En local, la liste des noms avec rappel des joueurs habituels. En ligne,
   c'est le **salon** : code de partie, QR code, bouton de partage et la liste des joueurs
   qui se remplit en direct au fur et à mesure des connexions.
3. **Quelles règles ?** *Classique* ou *Française*, chacune résumée en deux lignes. Un lien
   **Personnaliser…** — et lui seul — révèle les colonnes de jeu et les montants de bonus :
   un débutant ne les voit jamais.

Puis **Commencer la partie**. C'est ce moment, et pas le premier score inscrit, qui fige
les règles et la liste des joueurs. En ligne, seul l'organisateur dispose du bouton ; les
autres voient « En attente du lancement par l'organisateur » et basculent automatiquement
sur la feuille au top départ.

**Partie rapide** (accueil) court-circuite tout : 2 joueurs sur cet appareil, règles
classiques, la feuille s'ouvre directement.

**Rejoindre** (accueil) demande le code de la partie dans un champ prévu pour — majuscules
automatiques, collage accepté — puis le prénom. Le plus simple reste de viser le QR code
avec l'appareil photo : le lien ouvre l'application directement sur la bonne partie.

## Jouer en ligne, chacun sur son téléphone

Deux façons de jouer :

- **En local** (par défaut) : un seul téléphone tient la feuille de toute la table. Rien
  n'est envoyé sur Internet, aucun compte, fonctionne hors connexion.
- **En ligne** : chaque joueur a sa feuille sur son propre téléphone et voit les scores
  des autres se remplir en direct. **Aucun compte n'est nécessaire.**

Les deux se mélangent : **n'importe quel appareil** de la partie en ligne peut ajouter des
joueurs supplémentaires depuis le salon (« ＋ Joueur sur mon téléphone »). Un parent tient
ainsi sa feuille et celle de son enfant ; lui seul peut les écrire, tout le monde les voit.

- Chacun ne modifie que **sa** colonne. L'organisateur ne fait pas exception : pour écrire
  chez un autre joueur connecté, il doit activer **Organiser** dans le bandeau de tour
  (les cases qu'il touche sont alors marquées ✱). Sans ce garde-fou, il remplissait sans
  le vouloir la case du joueur dont c'était le tour.
- Chaque appareil peut tenir **plusieurs feuilles** : la sienne et celles des joueurs sans
  téléphone assis à côté. Elles sont éditables en permanence par leur porteur, puisque
  personne d'autre ne les tient.
- **Absences** : chaque appareil se signale toutes les 25 secondes. Un joueur silencieux
  depuis plus d'une minute apparaît « hors ligne » (dans la feuille, le bandeau et la liste
  des joueurs) et tout le monde en est averti ; celui qui touche **Quitter** apparaît
  « a quitté ». L'organisateur peut alors le **retirer de la partie** (croix ✕ dans le
  panneau **QR**), y compris en cours de jeu : le tour passe au joueur suivant.
- Un bandeau sous la feuille indique l'état de la connexion, le code de la partie, le
  nombre de joueurs et le nombre d'absents. Le bouton **QR** le ré-affiche à tout moment.
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

Le code est hébergé sur [github.com/Fofuls50/yahtzee](https://github.com/Fofuls50/yahtzee).
Pour activer (ou vérifier) la publication :

1. Sur le repo → **Settings → Pages**.
2. Sous *Build and deployment* → *Source*, choisir **Deploy from a branch**, puis la branche
   `main` et le dossier `/ (root)`. Cliquer sur **Save**.
3. Attendre ~1 minute : le site est en ligne sur `https://fofuls50.github.io/yahtzee/`

Pour publier une mise à jour, depuis ce dossier :

```bash
git add . && git commit -m "message" && git push
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

## Confort de jeu

Deux options dans les **Réglages** (roue dentée), actives par défaut et propres à chaque appareil :

- **Feuille sur un seul écran.** La feuille est mesurée après chaque rendu ; si elle tient
  déjà (tablette, grand écran), rien n'est modifié. Sinon l'application gagne de la place
  par étapes, de la moins gênante à la plus visible :
  1. elle masque le rappel d'aide en bas de page et resserre les marges autour de la feuille ;
  2. elle **resserre les lignes** (52 px → 34 px), sans toucher à la taille du texte ;
  3. si l'écart reste important, elle retire le **rappel de règle** sous chaque nom de
     combinaison (qui serait de toute façon illisible une fois réduit) et resserre les
     lignes davantage — le texte, lui, garde presque sa taille normale ; la règle complète
     reste accessible en touchant le nom ;
  4. en dernier recours elle réduit l'ensemble d'un coup (une transformation CSS : lignes et
     texte rétrécissent ensemble, rien ne se désaligne), en s'arrêtant à 55 % pour rester
     lisible. Au-delà — beaucoup de joueurs *et* plusieurs colonnes *et* règles françaises —
     la page redevient défilable.

  La hauteur totale étant affine en hauteur de ligne, deux mesures suffisent à calculer
  directement la valeur qui remplit l'écran : pas de tâtonnement, pas de scintillement.
- **Fenêtres (réglages, statistiques, assistant, joueurs).** Elles se ferment de trois
  façons : la croix en haut à droite, le bouton en bas, ou en tirant la poignée vers le bas
  comme dans une application. Chaque réglage n'affiche que son titre et son interrupteur ;
  le **?** déplie l'explication détaillée à la demande.
- **Retour à l'accueil uniquement par le logo** en haut à gauche. Le « tirer pour
  rafraîchir » du navigateur est neutralisé (`overscroll-behavior`), et un rechargement en
  cours de partie rouvre directement la feuille au lieu de l'écran d'accueil.
- **Garder l'écran allumé.** Utilise l'API *Wake Lock* : le téléphone ne se met plus en
  veille tant que la feuille est affichée, ce qui évite de le rallumer à chaque tour. Le
  verrou est relâché dès que l'onglet passe en arrière-plan et repris au retour. Nécessite
  Safari 16.4+ sur iPhone ; l'option est grisée si le navigateur ne la propose pas.

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
