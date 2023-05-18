# [FEEDBACK] Apprenant 3

> Bonjour 'apprenant 3', voici un compte-rendu détaillé de ton projet.
Je vais te donner autant de renseignements que possible afin que tu puisse améliorer et compléter ton projet


## Le fichier d'exemple `.env.example`

Concernant ton fichier `.env.example` ('/.env.example'), il manque une information importante pour l'installation de ton projet, en effet, le projet utilisant le middleware (extension)`express-session`, il est nécessaire d'avoir un paramètre `SESSION_SECRET` dans ton `.env` 
car dans ton code tu configurais le middleware de cette façon:

```js
app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: true,
  cookie: { }
}))
```

Ici, le `process.env` c'est le point d'entrée de ton fichier `.env`

Dans ton code je constate que tu appel le paramètre `SESSION_SECRET`

donc dans ton fichier .env tu dois ajouter `SESSION_SECRET`.

Nous passons donc de :

```env
PG_URL=postgresql://user:password@localhost/triple_triad
PORT=1234
```

vers :

```env
# DATABASE
PG_URL=postgresql://user:password@localhost/triple_triad

# EXPRESS
PORT=1234
SESSION_SECRET=9199cb4f-973e-4ad3-ae35-2c0b2b641ccd
```

tu constateras aussi l'ajout de commentaires pour garder une structure assez clean et facile à comprendre.

## Accueil
---

Au niveau de la liste des cartes je remarque que pour chaque carte, tu as 3 fois le nom de la carte qui remonte.
Il aurait été mieux de mettre le lien vers le detail de la carte directement sur la carte pour éviter un décalage au niveau du design et donc de supprimer le lien portant le nom de la carte sur la gauche.

Le lien pour ajouter une carte au deck pointe aussi vers le detail d'une carte et non sur l'ajout au deck.

La liste remonte donc c'est quand même positif.


## Détail d'une carte
---
Lorsque tu cliques sur le lien de détail d'une carte, une erreur se produit avec `card is not defined`.
L'erreur est produite car la variable portant la liste des cartes ne s'appelle pas `card` mais `one Card`.
il faut donc aller dans un premier temps sur le fichier `maincontroller` et il faut modifier `onecard` et le remplacer par `Card`.

Maintenant la variable "card" est utilisable dans la vue (cardDetail.ejs).

Il y a maintenant un autre probleme. tu utilise une boucle for sur 'card' :

`<% for (const singleCard of card) { %>`

alors que `card` est un object et non une liste d'objet.

il faut donc modifier le fichier ejs. 
Voici un exemple:

```js
<div class="card">
    <a href="/card/<%= card.id %>"><%= card.name %>
        <img  src="/visuals/<%=card.visual_name%>" alt="<%= card.name %> illustration">
        <p> ...</p>
        <p> ...</p>
    </a>
    <a class="link--addCard" title="Ajouter au deck" href="/card/<%= card.id %>">
        <%= card.name %>[ + ]
    </a>
</div>
```

l'appel vers l'image était aussi incorrect.

## La recherche
---

Au niveau de la recherche il y a plusieurs problèmes à corriger pour faire fonctionner les choses.
Dans un premier temps il faut aller dans `searchcontroller` et il faut ajouter l'import de data Mapper car tu l'utilises ensuite mais il n'est pas encore importé dans le fichier.

`const dataMapper = require("../dataMapper");`

Il y a aussi une coquille dans la construction de la méthode `search Element`, il faut remplacer

```js
searchElement :async (request, response, next) => {
```

par 

```js
searchElement: async (request, response, next) => {
```

pour rester sur un code propre.

ensuite tu appelles `dataMapper.cardElement` mais la methode n'existe pas dans ton fichier `dataMapper`.

il vaut mieux appeler `dataMapper.getCardByElement`

Enfin ta vue (main/element) ne correspond pas au résultat de la recherche. Ce que tu souhaites afficher c'est une liste, tout comme sur l'accueil.

Reprends ton code EJS de l'homepage et utilise le comme route pour ton résultat de recherche.


## Summary
---

En globalités tu as essayé de faire ce qu'il faut mais il semble que tu as eu pas mal de difficulté, notamment sur la compréhension du JS.

Il faudrait que nous prévoyions une visio afin de discuter pour revoir certaines bases. tu as un problème de compréhension concernant les objets, les arrays et une difficulté sur la syntaxe de template EJS.

Je me ferais un plaisir de t'aider à terminer ton application au maximum afin que tu assimiles au mieux les choses.

Tu n'as pas mis en place l'ajout au deck, la suppression et tu n'as donc pas fait les bonus.

Le plus important c'est de voir en priorité les bases avant d'en faire trop. N'hésite pas à revenir vers moi au plus vite afin que je t'aide à comprendre tes erreurs et à t'amener vers la réussite de ton projet.
