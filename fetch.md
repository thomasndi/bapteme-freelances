# Fetch

> Hello 'apprenant 1', voici quelques informations à propos du fetch.


le fetch permet d'envoyer une requête asynchrone au serveur afin d'obtenir une réponse sans avoir à recharger la page.
le fetch est très utilisé dans le monde du dev.

tu as sans doute déja entendu parler de la notion d'API REST ? 

Une API REST est un serveur, une application, un petit peu comme ton projet sauf qu'il n'y a aucune interface graphique.

Imagine le projet que tu as réalisé mais découpé en deux.

Un premier projet qu'on appelle frontend contenant uniquement les vues et la partie visible de l'application
et un deuxième projet  qu'on appelle backend contenant uniquement le router, la base de données et les controllers.

Grace au fetch, tu pourras depuis le projet frontend, aller télécharger les différentes données du projet backend pour les afficher à l'écran à travers des routes et le tout sans avoir à recharger ta page.

cette méthode permet de moderniser les applications web et les rendre beaucoup plus réactives.

Voici un exemple pour charger toutes tes cartes avec le fetch:

1) création de la methode fetch

```js

let response = await fetch(/data/cards.json);
const cards = response.json();

```

La variable card contient maintenant l'ensemble des cartes et tu peux l'utiliser pour afficher les cartes.

Si tu as besoin de plus d'informations contacte-moi pour un exemple en direct et pour que je t'apporte mon aide.
Il existe aussi des packages pour faciliter le fetch qui est utilisé par les plus grosses applications existantes:

Axios par exemple.
