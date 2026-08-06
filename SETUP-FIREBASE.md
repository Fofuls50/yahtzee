# Configuration Firebase — parties en direct

À faire une seule fois, ~20 minutes. Tout est gratuit et le reste : l'offre Spark de Firebase
ne se met pas en pause et ne demande aucune carte bancaire.

À la fin, tu me donnes le petit bloc de configuration de l'étape 5 et j'intègre le reste.

---

## 1. Créer le projet

1. Aller sur <https://console.firebase.google.com> et se connecter avec ton compte Google.
2. **Créer un projet** → nom : `yams` (le nom réel importe peu).
3. Google Analytics : **désactiver**, ça n'apporte rien ici et évite des écrans en plus.
4. Attendre la création, puis **Continuer**.

## 2. Activer la connexion anonyme

C'est ce qui permet aux joueurs de rejoindre sans compte.

1. Menu de gauche → **Créer** → **Authentication** → **Commencer**.
2. Onglet **Sign-in method** → dans la liste, choisir **Anonyme**.
3. Basculer l'interrupteur sur **Activer**, puis **Enregistrer**.

> La connexion Google n'est **pas** nécessaire pour l'instant : personne n'est obligé d'avoir
> un compte. On pourra l'ajouter plus tard sans rien casser, et un joueur anonyme pourra
> alors rattacher sa partie à son compte sans rien perdre.

## 3. Créer la base temps réel

Attention : il existe deux bases chez Firebase. Il nous faut la **Realtime Database**,
pas Firestore.

1. Menu de gauche → **Créer** → **Realtime Database** → **Créer une base de données**.
2. Emplacement : **Belgium (europe-west1)** — le plus proche, donc le plus rapide.
3. Règles de sécurité : choisir **Démarrer en mode verrouillé**. On les remplace juste après.

## 4. Poser les règles de sécurité

Onglet **Règles**, remplacer tout le contenu par ceci, puis **Publier** :

```json
{
  "rules": {
    "games": {
      "$gid": {
        ".read": "auth != null",
        ".write": "auth != null && (!data.exists() || data.child('host').val() === auth.uid)",

        "players": {
          "$pid": {
            ".write": "auth != null && (!data.exists() || data.child('uid').val() === auth.uid)"
          }
        }
      }
    }
  }
}
```

Ce que ces règles appliquent, et qui correspond à ton choix « chacun la sienne, l'hôte partout » :

| Qui | Peut faire |
|---|---|
| N'importe quel joueur connecté (même anonyme) | lire la partie, donc voir les scores en direct |
| Un joueur | écrire **uniquement** dans sa propre colonne |
| Un joueur qui arrive | créer sa colonne, mais pas s'emparer de celle d'un autre |
| L'hôte | tout modifier : corriger une erreur, tenir la colonne d'un enfant sans téléphone |
| Une personne sans le lien de la partie | rien : l'identifiant de partie est imprévisible |

## 5. Récupérer la configuration

1. Roue dentée en haut à gauche → **Paramètres du projet**.
2. Descendre jusqu'à **Vos applications** → icône **`</>`** (Web).
3. Surnom : `yams-web`. **Ne pas** cocher Firebase Hosting (le site reste sur GitHub Pages).
4. **Enregistrer l'application**.
5. Un bloc `firebaseConfig` s'affiche. **Copie-le et donne-le-moi.**

Il ressemble à ceci :

```js
const firebaseConfig = {
  apiKey: "AIza…",
  authDomain: "yams-xxxx.firebaseapp.com",
  databaseURL: "https://yams-xxxx-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "yams-xxxx",
  …
};
```

> Ces valeurs sont **publiques par conception** : elles figurent dans le code de tous les
> sites utilisant Firebase. Ce ne sont pas des mots de passe. Ce qui protège les données,
> ce sont les règles de l'étape 4. Tu peux donc me les transmettre et les publier sur GitHub
> sans crainte.

## 6. Autoriser ton site

Sans cette étape, la connexion échouera une fois le site en ligne.

1. **Authentication** → onglet **Settings** → **Domaines autorisés**.
2. **Ajouter un domaine** → saisir `<ton-pseudo>.github.io`.

`localhost` y est déjà, ce qui permet de tester en local.

---

## Ce que ça coûte, concrètement

L'offre gratuite couvre 100 connexions simultanées, 1 Go de stockage et 10 Go de trafic
par mois. Une partie de Yam's pèse quelques kilo-octets et mobilise autant de connexions
que de joueurs. Il faudrait des milliers de parties par mois pour s'en approcher.

## Vie privée

Les prénoms et les scores des parties en ligne sont stockés chez Google, en Belgique.
Les parties terminées sont effacées automatiquement par l'application au bout de 30 jours.

Le **mode local reste inchangé** : jouer sur un seul téléphone n'envoie toujours rien sur
Internet, ne demande aucun compte et fonctionne hors connexion.

## Supprimer une partie tout de suite

**Realtime Database** → onglet **Données** → survoler la ligne `games` → la croix rouge
efface tout. Sans conséquence : l'historique et les statistiques sont sur les téléphones.
