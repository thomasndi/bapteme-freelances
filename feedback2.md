# [FEEDBACK] Apprenant 2

> Bonjour 'apprenant 2', voici un compte-rendu détaillé de ton projet.
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

La liste des cartes remonte bien, c'est cool !
Si jamais la liste de cartes était bien plus longue il aurait été important de mettre
en place une pagination pour ne pas impacter les performances de la page.

RAS, rien à dire, la remontée de la liste fonctionne bien.

## Détail d'une carte
---

L'accès au détail des cartes fonctionne bien en revanche il manque quelques informations.
à la place d'afficher directement le niveau, l'élément, le nom, il faudrait ajouter un label avant la valeur pour savoir à quoi correspond la valeur.
par exemple;

élement: (terre, feu, ...)
level: (1, 2, ...)

c'est dans le fichier `card.ejs` (/app/views/card.ejs) qu'il faut apporter une modification.

## La recherche
---

La recherche par élement fonctionne assez bien sauf si on sélectionne "aucun element", en effet, une liste remonte pour chaque élément sélectionné.

pour corriger le problème, il faut se rendre dans le fichier `searchController` (/app/controllers/searchController) et modifier `getSearchResults`.
Actuellement le result est construit sur `data Mapper.GetElements` mais impossible de faire une recherche sur "aucun element" basé sur une liste avec uniquement des éléments.
Il faut donc plutôt baser la recherche sur la liste des cartes complète donc sur `data Mapper.GetAllCards`

par contre, il est important d'avoir une consistance pour ne pas perdre les utilisateurs de tes projets.
Sur ton accueil il y a une vue liste de cartes en mode grille et lors d'une recherche tu affiches les elements en mode liste textuelle.
il aurait été bien d'avoir une grille de carte comme sur l'accueil, mais filtrer via ta recherche.

## Construire un deck
---

Au niveau de la construction d'un deck, l'ajout fonctionne donc c'est déjà un bon point.

J'ai remarqué par contre qu'il est possible d'ajouter plus de 5 cartes dans le deck et c'est un bug car la feature indique qu'il est possible seulement d'avoir 5 cartes max.

regarde ta méthode `adddeck` dans le `deckcontroller` au niveau de ta condition suivante;

```js
if (!foundDeckCard) {
  req.session.deck.push(deckCard);
}
```

il serait judicieux d'ajouter du code pour bloquer l'ajout d'une carte si le deck contient déjà 5 cartes.

```js
if (!foundDeckCard && req.session.deck.length < 5) {
  req.session.deck.push(deckCard);
}
```

Il y a aussi un autre petit problème, l'élément des cartes ne remonte pas dans le deck.
à la place c'est un "$' qui remonte.

en effet dans ton deck.ejs (app.views/deck.ejs) tu as la ligne suivante:

```js
<p class="item-subtotal">$<%= card.price%></p>
```

retire la coquille ($) et remplace card.price par card.element (price existe pas dans l'objet card).

```js
<p class="item-subtotal"><%= card.element%></p>
```

il manque la suppression d'une carte au deck par contre


## Summary
---

Tu as fait de ton mieux pour atteindre les features nécessaires au bon fonctionnement de l'application mais il semblerait que tu as eu quelques difficultés.
J'ai essayé de t'aguiller dans la bonne direction avec ce feedback. Continue tes efforts, tu es proche de la fin.
Ce serait bien qu'on fasse une session visio en partage d'écran afin que je t'apporte plus d'aide si tu en ressens les besoins.
Continue à travailler et si je peux te donner un conseil. Écrit toi une liste des tâches clés que tu dois accomplir, afin de checker et de vérifier l'ensemble des points avant de rendre ton projet.
