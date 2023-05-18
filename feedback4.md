# [FEEDBACK] Apprenant 4

> Bonjour 'apprenant 4', voici un compte-rendu détaillé de ton projet.
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

## Accueil et détail
---

La liste des cartes remonte bien. C'est très positif.
C'est propre, ça fonctionne.

Par contre, le lien vers le detail d'une carte ne fonctionne pas, tu as mis un lien vers la page actuelle à la place.

pour corriger le problème, modifie le href de la carte de

```js
<a href="#">
```

vers

```js
<a href="/card/<%= card.id %>">
```

Ensuite il va falloir te rendre dans ton contrôler pour modifier ta méthode `cardPage`

```js
cardPage: async (req, res) =>{
    try {
      const card = await dataMapper.getCard();
      res.render('card',{
      
      });
    } catch(error) {
      console.error(error);
      res.status(500).send(`An error occured with the database :\n${error.message}`);
    }
  }
```

Il faut que tu passes un ID à `data Mapper.GetCard` car tu veux accéder à une carte spécifique.
pour ce faire tu utilises le paramètre `req` (request) de la méthode. Il faut aussi passer ta carte et le titre vers la vue.

```js

cardPage: async (req, res) =>{
    try {
      const card = await dataMapper.getCard(req.params.id);
      res.render('card',{
        title: card.name,
        card
      });
    } catch(error) {
      console.error(error);
      res.status(500).send(`An error occured with the database :\n${error.message}`);
    }
  }
```

tu vas devoir modifier aussi ton data Mapper pour passer l'id de ta carte en paramètre à la méthode `getCard`

```js
getCard: async (id) => {
    //const targetId = Number(request.params.id);
    const query =
    `
      SELECT *
      FROM "card"
      WHERE "id" = ${id}
    `
    const result = await client.query(query);

     if (result.rowCount === 1) {
      return result.rows[0] 
      } 
      return null;
  },
```

Enfin il faut modifier ton fichier EJS `card` car les valeurs que tu lui passes sont maintenant "card" et "title".

card est un objet et non une liste de cartes.

tu peux donc enlever la boucle for.

tu passes donc de

```js
 <% cards.forEach( (card) => { %>

    <div class="card">
      <a href="#">
        <img src="/visuals/<%= card.visual_name %>" alt="<%= card.name %> illustration">
        <p class="cardName"><%= card.name %></p>
      </a>
      <a class="link--addCard" title="Ajouter au deck" href="#">[ + ]</a>
    </div>

  <% }) %>
```

vers

```js
    <div class="card">
      <a href="#">
        <img src="/visuals/<%= card.visual_name %>" alt="<%= card.name %> illustration">
        <p class="cardName"><%= card.name %></p>
      </a>
      <a class="link--addCard" title="Ajouter au deck" href="#">[ + ]</a>
    </div>
```

Maintenant la liste complète des cartes ainsi que le detail fonctionne.
Pour aller plus loin, ajoute les différents stats de ta carte via le fichier EJS maintenant que tu as accès à ta carte.


## Summary
---

Globalement tu as principalement focus sur le visuel, ta page d'accueil est propre, la liste des carte remonte. Focus sur la fonctionnalité plutot que sur l'apparance.
Néemoins, il manque des features clé comme la recherche, les decks et tu as eu des difficultée pour accéder au détail de tes cartes.

Je t'invite à me contacter pour discuter ensembles que je puisse t'aider au niveau de tes difficultée pour comprendre et finir ton projet.

