---
title: Session 1 - Introduction à HTML, CSS et Javascript
excerpt: ""
order: 1
---


Une page web, c'est-à-dire toute page d'un site internet consultable à
l'aide d'un navigateur web, est composée de plusieurs éléments. Son contenu,
c'est-à-dire l'ensemble du texte, des liens, du contenu multimédia,
est décrit dans un fichier HTML (pour Hypertext Markup Language). Ce
fichier décrit aussi l'organisation logique du contenu (qu'est-ce qui est
un titre, qu'est-ce qui est un menu, une section, une liste, etc.). La façon
dont ce contenu est affiché (emplacement, taille, couleur, etc. de chaque
élément) est décrite dans un ou plusieurs fichiers de style CSS (pour
Cascading Style Sheets).

Ces deux éléments suffisent à décrire une page web dite "statique" (dont
le contenu ne change pas). Cependant, de nos jours la plupart des pages web
que nous consultons sont "dynamiques" (leur contenu ou la façon dont il est
affiché varie au cours du temps et/ou en fonction des actions de
l'utilisateur). De bons exemples de pages web dynamiques sont une messagerie
instantanée ou un réseau social (les interactions telles qu'envoyer un
message, commenter ou réagir à une publication ou charger plus de contenu
une fois arrivé à la fin du contenu déjà chargé ne demandent pas de changer
de page). Ceci est fait grâce à un langage de programmation interprétable
par les navigateurs internet appelé Javascript.

1. TOC
{:toc}

# Description du contenu : HTML

Une page web est décrite dans un document HTML comme un arbre : un élément
peut contenir d'autres éléments, qui eux-mêmes peuvent contenir d'autres
éléments, etc. Cette structure d'arbre est appelée DOM (pour Document
Object Model). Dans le reste de ce cours, nous utiliserons la
dernière version du HTML, le HTML 5.

## Structure générale d'une page

Un élément peut être du texte ou une balise (vide ou non-vide). Les
balises peuvent avoir des attributs (par exemple l'URL de l'image voulue
pour une balise image). Les balises non-vides peuvent elles-mêmes contenir
des éléments (textes et/ou balises). Voici un exemple minimal que nous
allons analyser :

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Titre de la page</title>
        <link rel="stylesheet" href="style.css">
        <script src="script.js"></script>
    </head>
    <body>
        <!-- contenu de la page -->
    </body>
</html>
```

- `<!DOCTYPE html>` est une balise vide de type '!doctype' marquée d'un
attribut sans valeur "html" qui doit être placée au tout début du fichier
pour indiquer au navigateur que notre fichier est au format HTML 5.
- `<html>` est une balise non-vide de type 'html' qui contient l'ensemble
de la page HTML. Tout ce qui est déclaré entre `<html>` et sa balise de
fermeture `</html>` est considéré comme appartenant à la page. Ce
comportement est le même pour toutes les balises non-vides. La norme
prévoit que le contenu de la balise <html> soit constiué d'une balise
non-vide de type 'head' qui contient toutes les informations qui ne sont
pas directement du contenu (titre, mots clés pour les moteurs de recherche,
lien vers les fichiers de style CSS ou les scripts Javascript, etc.) ainsi
que d'une balise non-vide de type 'body' qui contient le réel contenu de la
page.
- `<meta charset="utf-8">` est une balise vide de type 'meta' qui indique
au navigateur la façon dont les caractères sont encodés dans le fichier
grâce à son attribut avec valeur 'charset' (qui a ici pour valeur la chaîne
de caractères "utf-8" qui est un type d'encodage).
- `<title>` est une balise non-vide dont le contenu est le titre de la page
(affiché par le navigateur sur l'onglet de la page et dans la barre de titre
quand l'onglet est actif, affiché par les moteurs de recherche quand ils
mettent un lien vers la page dans leurs résultats).
- `<link rel="stylesheet" href="style.css">` est une balise vide indiquant
que le fichier de style CSS "style.css" doit être chargé pour afficher la
page correctement.
- `<script src="script.js">` est une balise non-vide indiquant que le
script "script.js" doit être chargé pour que la page se comporte
correctement.
- `<!-- contenu de la page -->` est un commentaire (démarré par `<!--` et
terminé par `-->`).

## Description du contenu

Pour le remplissage du contenu de la page, le plus simple est d'étudier
un autre exemple. Le code suivant est un exemple de contenu pour la balise
`<body>`. Une version éditable avec aperçu pour l'expérimentation est
disponible ici :
[http://tiny.cc/SARhtml](http://tiny.cc/SARhtml)

```html
<h1>
  Ma première page web (titre niveau 1)
</h1>
<p>
  Ceci est un paragraphe.
</p>

<h2>
  Les balises basiques (titre niveau 2)
</h2>
<h3>
  Retour à la ligne et lien (titre niveau 3)
</h3>
<p>
  Un autre paragraphe suivi d'un retour à la ligne (br) puis d'un lien vers Google.
  <br>
  <a href="https://www.google.fr/">Texte du lien</a>
</p>
<h3>
  Image
</h3>
<img src="https://www.imt-atlantique.fr/sites/default/files/users/user336/IMT%20Atlantique%20-%203%20campus.png">
<h3>
  Mise en forme de texte (déconseillé)
</h3>
<p>
  Il est plutôt conseillé d'utiliser le CSS pour la mise en forme du texte. Voici cependant des
  balises de mise en forme rapide pour le prototypage : <b>ceci est en gras</b>, <i>ceci est en
  italique</i> et <u>ceci est souligné</u>.
</p>

<h2>
  Les listes
</h2>
<p>
  Les listes peuvent être ordonnées (les éléments sont alors numérotés) ou non-ordonnées (les
  éléments sont en général précédés d'un tiret ou d'une bulle).
</p>
<h3>
  Listes ordonnées (ordered list: ol)
</h3>
<ol>
  <li>Premier élément</li>
  <li>Deuxième élément</li>
</ol>
<h3>
  Listes non-ordonnées (unordered list: ul)
</h3>
<ul>
  <li>Premier élément</li>
  <li>Deuxième élément</li>
</ul>

<h2>
  Les tableaux
</h2>
<p>
  Les tableaux sont contenus dans des balises "table". Une balise contient plusieurs balises
  "tr", une pour chaque ligne du tableau. Enfin, chaque balise "tr" contient des balises "th"
  ou "td" : une par colonne. Une balise "th" indique une cellule de titre (header), tandis que
  "td" indique une cellule normale. Par défaut, les cellules n'ont pas de bordure, mais cela peut
  être changé en CSS.
</p>
<table>
  <tr>
    <th>Nom</th>
    <th>Âge</th>
    <th>Pays</th>
  </tr>

  <tr>
    <td>Maria</td>
    <td>25 ans</td>
    <td>Espagne</td>
  </tr>
  <tr>
    <td>Nicolas</td>
    <td>19 ans</td>
    <td>France</td>
  </tr>
</table>
```

## Formulaires

Les formulaires permettent à l'utilisateur d'entrer des informations pour
qu'elles soient traitées par le site web. Ils peuvent servir à se connecter
à un site, à créer un compte, à faire un virement bancaire, à envoyer un
message instantané ou un commentaire, etc.

Un formulaire est composé de plusieurs entrées (input) :
des champs de texte, menus déroulants, cases à cocher etc.
Ces entrées sont en général assorties d'une annotation textuelle (label)
permettant à l'utilisateur de savoir ce qu'on lui demande.
Enfin, un formulaire contient la plupart du temps un
bouton de soumission de formulaire.

Il est en outre possible de signaler au navigateur quel est le type de
réponse attendue pour un champ de formulaire (par exemple s'il est
indispensable de remplir un champ, s'il doit contenir une adresse
e-mail ou une date valide, si c'est un mot de passe qu'il faut donc
cacher, etc.). Cela permet au navigateur de proposer une méthode
d'entrée adaptée (par exemple un calendrier pour une date) et d'éviter
la soumission du formulaire si son contenu était de toute façon rejeté.

À la soumission, les valeurs entrées par l'utilisateur sont envoyées au
serveur à une URL précisée dans l'attribut "action" de la balise de
formulaire `<form>` qui englobe tout le formulaire. Nous verrons dans les
sections suivantes ce que cela signifie et comment ces données sont
ensuite traitées.

Voici un exemple de formulaire. Une version éditable avec aperçu pour
l'expérimentation est disponible ici :
[http://tiny.cc/SARform](http://tiny.cc/SARform)

```html
<!-- Soumission à la page /signup par méthode POST -->
<form action="/signup" method="post">
  <p> <!-- On met chaque entrée distincte dans un paragraphe -->
    <label>Titre</label><br>
    <label>
      <!-- name : nom qui sera utilisé par le serveur
                  pour référer à cette valeur
           value : valeur donnée en sélectionnant cette
                   case -->
      <input type="radio" name="title" value="m">
      M.
    </label>
    <label>
      <input type="radio" name="title" value="mme">
      Mme.
    </label>
    <label>
      <input type="radio" name="title" value="np">
      Autre/non-précisé
    </label>
  </p>
  <p>
    <label>Prénom</label><br>
    <input type="text" name="prenom">
  </p>
  <p>
    <label>Nom</label><br>
    <input type="text" name="nom">
  </p>
  <p>
    <label>Adresse e-mail</label><br>
    <!-- required indique que ce champ est requis -->
    <input type="email" name="email" required>
  </p>
  <p>
    <label>Numéro de téléphone</label><br>
    <input type="tel" name="telephone">
  </p>
  <p>
    <label>Mot de passe</label><br>
    <input type="password" name="mdp">
  </p>
  <p>
    <label>Pays</label><br>
    <select>
      <option>France</option>
      <option>Espagne</option>
      <option>Etats-Unis</option>
      <option>Italie</option>
      <option>Allemagne</option>
    </select>
  </p>
  <p>
    <label>
      <input type="checkbox" value="cu">
      J'accepte les <a href="/cu">conditions d'utilisation</a>
    </label>
  </p>
  <p>
    <button type="submit">Créer mon compte</button>
    <button type="reset">Réinitialiser</button>
  </p>
</form>
```


# Description du style (CSS)

Une feuille de style CSS est un ensemble de règles d'affichage qui
portent sur les éléments de la page web. On peut changer l'organisation
générale des éléments (position sur la page) ou le style même des
éléments (couleur, marges, police de texte, etc.).

Pour pouvoir cibler une partie précise de la page web, il est souvent
nécessaire d'ajouter des balises de ciblage. Les deux balises de ciblage
principales sont `<div>` et `<span>`. La différence entre les deux est
que par défaut `<div>` désigne un bloc du site tandis que `<span>`
désigne un bout de texte (il peut donc être utilisé à l'intérieur
d'une phrase).

Une règle CSS se présente sous la forme :
```
sélecteur {
  attribut1: valeur1;
  attribut2: valeur2;
  ...
}
```

Le sélecteur désigne quels éléments seront affectés par la règle,
tandis que les attributs et leur valeur définissent le style à appliquer.

Dans un premier temps nous nous limiterons à trois types de sélecteurs  CSS :
* les sélecteurs de balise
* les sélecteurs de classe
* les sélecteurs d'identifiant

## Règles de balise

Une règle de balise porte sur toutes les balises d'un type précis. Le
sélecteur est alors simplement le type de balise à cibler. Dans
l'exemple suivant, nous imitons une application de messagerie instantanée
en séparant les paragraphes les uns des autres de 26 pixels, et nous
donnons au texte des paragraphes (entourés d'une balise `<span>`) un fond
gris clair arrondi de 16 pixels, avec une marge interne (distance entre le
bord de la couleur de fond et le texte) de 12 pixels. De plus, nous demandons
au navigateur d'essayer plusieurs polices dans l'ordre donné (selon les
systèmes d'exploitation, tout le monde n'a pas les mêmes polices installées).

Exemple interactif : [http://tiny.cc/SARcss1](http://tiny.cc/SARcss1)

HTML :

```html
<p>
  <span>
    Salut !
  </span>
</p>
<p>
  <span>
    Ça va ?
  </span>
</p>
```

CSS :

```css
p {
  margin: 26px;
}

span {
  background: #DADADA;
  border-radius: 16px;
  padding: 12px;
  font-family: Helvetica Neue, Segoe UI, Helvetica, Arial, sans-serif;
}
```

## Règles de classe

Une règle de classe cible des éléments qui ont déclaré une
classe particulière dans leurs attributs. Ces règles sont généralement
préférées aux règles de balise. Le sélecteur commence par un point, suivi
du nom de la classe à cibler. Dans l'exemple suivant, les
deux premiers "span" ont la classe "message" tandis que le dernier a
la classe "message" et la classe "by-me".

Exemple interactif : [http://tiny.cc/SARcss2](http://tiny.cc/SARcss2)

HTML :

```html
<p>
  <span class="message">
    Salut !
  </span>
</p>
<p>
  <span class="message">
    Ça va ?
  </span>
</p>
<p>
  <span class="message by-me">
    Oui et toi ?
  </span>
</p>
```

CSS :

```css
p {
  margin: 26px;
}

.message {
  background: #DADADA;
  border-radius: 16px;
  padding: 12px;
  font-family: Helvetica Neue, Segoe UI, Helvetica, Arial, sans-serif;
}

.by-me {
  float: right;
  background: #25B;
  color: white;
}
```

## Règles d'identifiant

Une règle d'identifiant cible un élément particulier d'une page.
Il est possible de déclarer un identifiant pour un élément HTML
en déclarant l'attribut "id" d'une balise. Chaque identifiant
doit être unique à l'ensemble de la page. Un sélecteur par 
identifiant est formé du symbole # suivi de l'identifiant.
Dans l'exemple suivant, la balise ```<div>``` a pour identifiant
"topbar".

Exemple interactif :
[http://tiny.cc/SARcss](http://tiny.cc/SARcss)

HTML :
```html
<div id="topbar">
  fakebook
</div>
<p>
  <span class="message">
    Salut !
  </span>
</p>
<p>
  <span class="message">
    Ça va ?
  </span>
</p>
<p>
  <span class="message by-me">
    Oui et toi ?
  </span>
</p>
```

CSS :
```css
#topbar {
  padding: 12px;
  background: #25B;
  font-family: sans-serif;
  color: white;
  font-size: 28px;
}

p {
  margin: 26px;
}

.message {
  background: #DADADA;
  border-radius: 16px;
  padding: 12px;
  font-family: Helvetica Neue, Segoe UI, Helvetica, Arial, sans-serif;
}

.by-me {
  float: right;
  background: #25B;
  color: white;
}
```


# Page dynamique (Javascript)

Javascript permet de rendre une page dynamique, c'est-à-dire
qu'on peut modifier son contenu au cours du temps et réagir à
des actions effectuées par l'utilisateur.

Le but ici n'est pas de comprendre comment fonctionne Javascript
(une session sera dédiée à cela), mais de comprendre son rôle et
comment il interagit avec la page HTML.

Pour cela, nous complétons l'exemple vu précédemment par un
formulaire de soumission de message. On indique au formulaire
via son attribut "onsubmit" qu'il faut exécuter une fonction
Javascript à la soumission du formulaire. Cette fonction se charge
de vérifier que le message entré n'est pas vide (si c'est le cas,
elle affiche un pop-up d'avertissement), ajoute le message dans
la page HTML et vide le champ de texte.

Exemple interactif :
[http://tiny.cc/SARjs](http://tiny.cc/SARjs)

Javascript :
```javascript
// Fonction appelée à la soumission du formulaire
function checkAndSend(message_form) {
  // On récupère le message entré par l'utilisateur
  message = message_form.message.value;
  if (message == "") {
    // On affiche un popup
    alert("Le message est vide !");
  } else {
    postMessage(message);
    // On vide le champ de texte
    message_form.message.value = "";
  }
  return false; // Ne pas soumettre le formulaire
}

function postMessage(message) {
  // Créer <p> et <span>
  var p_node = document.createElement("p");
  var span_node = document.createElement("span");
  // Ajouter les classes à <span>
  span_node.classList.add("message");
  span_node.classList.add("by-me");
  // Créer le texte
  var text_node = document.createTextNode(message);
  // Insérer le texte dans <span> et <span> dans <p>
  span_node.appendChild(text_node);
  p_node.appendChild(span_node);
  // Ajouter <p> dans le <div> avec l'id "messages"
  document.getElementById("messages").appendChild(p_node);
}
```
