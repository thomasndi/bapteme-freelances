# [FEEDBACK] Apprenant 1

> Bonjour 'apprenant 1', voici un compte-rendu détaillé de ton projet.
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

Au niveau du détail des cartes, ça fonctionne bien !
Les informations remontent bien, donc ce n'est pas mal...
Au niveau de ton `dataMapper` (./app/dataMapper.js) j'aurais fait une modification.

```js
const query =
    `
        SELECT *
        FROM "card"
        WHERE "id" = $1
    `;

const result = await database.query(query, [id]);
```

Il est préférable d'utiliser la syntaxe suivante (Template literals) pour ajouter du texte avec des paramètres ou expressions à l'intérieur.
Entre les crochets, tu peux écrire des expressions directement en JS ou bien de passer des variables.

```js
const query =
    `
      SELECT *
      FROM "card"
      WHERE "id" = ${id}
    `
const result = await database.query(query);
```

Pour le reste c'est bon pour moi.

## La recherche
---

La recherche par élément fonctionne plutôt bien;
la seule chose que je puisse reprendre c'est le texte présent à la suite du `résultat de la recherche` qui est égal à `Nan` peut importe le type d'élément que l'on cherche.

En effet le problème est situé au niveau du `title` dans `searchController` (./app/controllers/searchController)

```js
 res.render('main/cardList', {
        cards: searchedResults,
        title: 'Résultat de la recherche : ' + (searchedElement === 'null' ? ' sans élément' : + searchedElement),
      });
```

Comme pour le détail d'une carte, je t'invite à utiliser le `template literals`, après quelques modifications le correctif est le suivant.

```js
 res.render('main/cardList', {
        cards: searchedResults,
        title: `Résultat de la recherche : ${searchedElement ? searchedElement : 'sans élément'}`
      });
```

Entre les backticks (``) on entre le texte "Résultat de la recherche: " puis l'expression présente ${ICI} c'est un ternary conditional operator.

${Est-ce que searchedElement existe ? alors on retourne searchedElement : Sinon on retourne 'sans élément'}

## Construire un deck
---

Au niveau de l'ajout et de la suppression de carte dans un deck, rien à dire.
Ça fonctionne bien.
J'ai inspecté les méthodes `addcard` et `removecard` et pour moi c'est ok.

## Bonus
---

Tu as su répondre à tous les éléments bonus.
J'ai pu tester les différentes recherches et tout fonctionne, au niveau du code aussi, c'est bon de mon coté.

## Summary
---

En globalité, tu as fait du très bon boulot.
Les features clés de l'application fonctionnent.
Pour l'architecture de l'app, tu aurais pu mieux organiser les choses, par exemple le fichier middleware dans le dossier middleware ce n'est pas fou, un rename en session js dans le dossier middleware aurait été mieux.
pareillement pour les fichiers relatifs au database, data mapper, c'est mieux de les placer dans ton dossier data.

Il y avait quelques petits bugfix mais globalement l'app fonctionne et tu as fait du bon boulot.

Je t'invite tout de même à assimiler le concept de `Ternary operator` et de  `Template literals` et de réfléchir à comment mieux organiser tes fichiers et dossiers dans ton projet.

Dans tous les cas, un grand bravo pour le travail accompli.
