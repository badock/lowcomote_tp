---
title: Session 9 - Pages dynamiques avec Javascript
excerpt: ""
order: 9
---


Jusqu'à maintenant nous avons utilisé Flask pour générer le contenu de
la page à chaque requête (notamment grâce aux templates). Cependant, une
fois la page envoyée à l'utilisateur, Flask n'a plus aucun contrôle sur
son contenu. Il faut donc utiliser une autre technologie pour rendre la
page dynamique, c'est-à-dire pour que son contenue évolue au cours du
temps ou en réponse à des actions de l'utilisateur.

Tous les navigateurs modernes courants supportent le langage de
programmation Javascript. Ce dernier permet de rendre une page dynamique
car il est possible d'exécuter du code au chargement de la page, de
manière périodique ou en réponse à des événements. Nous utiliserons
aussi la bibliothèque Javascript [jQuery](https://jquery.com/) qui rend
beaucoup plus intuitive et sûre la programmation en Javascript.

Au cours de cette session, nous développerons progressivement une
application web permettant d'afficher en temps réel les prochains
passages aux arrêts de transports en commun de la TAN grâce à l'API
publique fournie par la TAN. Ce sera l'occasion de nous familiariser
avec le format d'échange de données [JSON](https://www.json.org/).
Notre application présentera aussi un thème clair et un thème sombre,
avec la possibilité de basculer entre les deux.

1. TOC
{:toc}

# Préparatifs

Veuillez d'abord télécharger et importer le projet suivant :
[archive](/assets/archives/horairesTAN.zip).
L'archive contient une application FLASK qui servira de proxy
pour appeler l'API de la TAN. Cette dernière n'autorise en effet
pas un navigateur à faire un appel depuis un site autre que celui
de la TAN. Pour plus d'information sur cette fonctionnalité
appelée Cross-Origin Resource Sharing (CORS), voir la
[page Wikipedia correspondante](https://fr.wikipedia.org/wiki/Cross-origin_resource_sharing).

L'archive contient :

- `app.py` : application Flask qui définit trois points d'entrée :
  * `/` : affichage de la page web (template `horaires.html.jinja2`)
  * `/liste_arrets` : proxy vers la liste des arrêts fournie par l'API de la TAN au format JSON
  * `/arrets/<code>` : récupère les prochains passages à l'arrêt dont le code est passé en URL auprès de l'API de la TAN, modifie le format des données pour le rendre plus facile à utiliser par notre application et renvoie le résultat au format JSON
- `templates/horaires.html.jinja2` : page HTML de notre application. Elle ne contient aucune information provenant de la TAN, tout sera dynamique.
- `static/css/hoaires.css` : feuille de style CSS très simple pour améliorer l'affichage de notre application
- `static/js/horaires.js` : script Javascript presque vide que nous allons compléter lors de la session

## Fichiers CSS / Javascript à inclure pour notre projet

Veuillez observer le contenu du fichier
`templates/horaires.html.jinja2`. Comme nous utilisons Bootstrap,
nous devons d'abord inclure le CSS de Bootstrap. Cependant, cette
fois nous n'allons pas utiliser le CSS de référence mais un thème
Bootstrap, qui est une version modifiée du CSS de référence. Nous
utiliserons le thème suivant :
[https://bootswatch.com/flatly/](https://bootswatch.com/flatly/).
Nous faisons cela car une version sombre de ce même thème existe :
[https://bootswatch.com/darkly/](https://bootswatch.com/darkly/).
Pour le moment, nous nous contentons d'inclure le thème clair :

```ħtml
<link id="theme" rel="stylesheet" href="https://bootswatch.com/4/flatly/bootstrap.min.css" crossorigin="anonymous">
```

Vous remarquerez que nous incluons aussi notre propre CSS dont l'URL est
un paramètre du template :

```ħtml
<link rel="stylesheet" href="{{ css_path }}">
```

Par défaut, Bootstrap nécessite d'inclure trois fichiers Javascript :
jQuery slim, Popper et Bootstrap lui-même. Nous remplaçons jQuery
slim par la version complète de jQuery car nous en aurons besoin.
Cela donne :

```html
<script src="https://code.jquery.com/jquery-3.2.1.min.js" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"
        integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q"
        crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js"
        integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl"
        crossorigin="anonymous"></script>
```

Nous importons aussi notre fichier Javascript, dont l'URL est un
paramètre du template :

```html
<script src="{{ js_path }}"></script>
```

## L'API de la TAN 

Pour notre applications, nous utiliserons l'API temps réel de la TAN :
[https://data.nantesmetropole.fr/explore/dataset/244400404_api-temps-reel-tan/information/](https://data.nantesmetropole.fr/explore/dataset/244400404_api-temps-reel-tan/information/).
Vous trouverez en suivant ce lien la description de l'API et une
documentation. Nous utiliserons deux points d'entrée proposés :
- [http://open.tan.fr/ewp/arrets.json](http://open.tan.fr/ewp/arrets.json)
qui donne la liste des arrêts complète
- [http://open.tan.fr/ewp/tempsattente.json/<code>](http://open.tan.fr/ewp/tempsattente.json/COMM)
qui donne la liste des prochains passage à un arrêt (lien : exemple de l'arrêt Commerce)

Ces deux points d'entrée nous renvoient des informations au format JSON.
Ce format permet l'échange de données structurées sous forme de
dictionnaires, tableaux, entiers, nombres à virgule, booléens et chaînes
de caractères. Voici un extrait de la liste des arrêts qui nous est
retournée par le premier point d'entrée :

```json
[
   {  
      "codeLieu":"CNAV",
      "libelle":"Chantiers Navals",
      "distance":null,
      "ligne":[  
         {  
            "numLigne":"1"
         }
      ]
   },
   {  
      "codeLieu":"CTRE",
      "libelle":"Chantrerie",
      "distance":null,
      "ligne":[  
         {  
            "numLigne":"C6"
         },
         {  
            "numLigne":"75"
         },
         {  
            "numLigne":"115"
         }
      ]
   },
   {  
      "codeLieu":"CHZY",
      "libelle":"Chanzy",
      "distance":null,
      "ligne":[  
         {  
            "numLigne":"C1"
         }
      ]
   }
]
```

On a donc un tableau de dictionnaire, chaque dictionnaire contenant les
informations d'un arrêt (son code de lieu, son nom réel, la distance qui
le sépare des coordonnées GPS passées en argument (`null` ici car nous
n'en avons pas fourni) et une liste dictionnaires contenant chacun un
numéro de ligne passant à cet arrêt). Cette syntaxe est identique à
celle utilisée en Python et en Javascript (sauf que `null` devient
`None` en Python).


# Thème sombre : les bases de Javascript + jQuery

Nous allons d'abord permettre à l'utilisateur de changer le thème de notre
application en cliquant sur le bouton à droite de la barre de navigation
(ou en bas en affichage mobile). Pour cela, nous changerons la feuille de
style CSS Bootstrap utilisée à chaque fois qu'on clique sur le bouton.

## Variables et fonctions en Javascript

Il y a deux façons de déclarer une variable en Javascript : avec le mot
clé `var` et avec le mot clé `let`. La différence est la portée : une
variable déclarée avec `var` sera accessible dans tout le script une fois
déclarée, alors qu'une variable déclarée avec `let` ne sera accessible que 
dans la portée de la déclaration (comme en Java). `let` a été introduit
plus récemment et est désormais considéré comme une meilleure pratique pour
éviter des bugs.

```javascript
let entier = 5;
let nvirgule = 2.5;
let texte = "Je suis un texte";
let tableau = [4, 2, 42];
let tableau2 = [1, "2", 3.4, tableau]; // Possible, mais à éviter
let objet = {champ1: "valeur", "champ2": 2}; // Nom des champs avec ou sans guillemets

let l = tableau.length; // 3
let pe = tableau[0]; // 4
let c1 = objet["champ1"]; // "valeur"
let c2 = objet.champ2;    // 2        (les deux sont interchangeables)

let test1 = 5=="5"; // Vrai en Javascript (conversion implicite)
let test2 = 5==="5"; // Faux
let test3 = 5===5; // Vrai

let b = true;
if (b) {
    let v_let = 5;
    var v_var = "Salut";
    console.log(v_let, v_var); // Équivalent de print, prend autant d'argument que désiré
}
console.log(v_var); // Marche seulement si b vaut true
console.log(v_let); // Erreur (c'est une bonne chose)
```

Comme vous pouvez le remarquer, Javascript permet de faire des choses
étranges, il faut donc être un programmeur rigoureux pour éviter les
bugs. Pour les curieux, et après le cours, cette vidéo est intéressante
(le Javascript est à partir de 1:20) :
[https://www.destroyallsoftware.com/talks/wat](https://www.destroyallsoftware.com/talks/wat)

Il y a plusieurs façons de déclarer une fonction en Javascript, les
deux que nous utiliserons sont :

```javascript
function maFonction(param1, param2) {
    // Contenu
    return 4; // return optionnel
}

let monAutreFonction = function(param1, param2) {
    // Contenu
    return 2; // return optionnel
}
```

On notera qu'on peut passer des fonctions en paramètre :

```javascript
function appliquer(f, a, b) {
    return f(a,b);
}

function sum(n1,n2) {
    return n1+n2;
}

console.log(appliquer(sum, 3, 2)); // Affiche 5
console.log(appliquer(
    function (n,m) {return n-m;},
    3,
    2
)); // Affiche 1
```

## Accès au DOM avec jQuery

jQuery nous permet de créer une référence vers les éléments de notre
page HTML avec les mêmes sélecteurs que CSS. Ainsi :

```javascript
$("p"); // Référence vers tous les paragraphes
$(".maclasse"); // Référence vers tous les éléments de classe "maclasse"
$("#monid"); // Référence vers l'élément d'ID "monid"
```

Une fois que l'on a une référence vers un ou plusieurs éléments du DOM,
on peut leur appliquer plusieurs opérations. Certaines opérations
peuvent s'appliquer sur plusieurs éléments, quand d'autres ne
s'appliquent que sur la première référence (on cherchera alors à ne
cibler qu'un seul élément avec notre sélecteur).

```javascript
// Plusieurs éléments
$("p").hide(); // Cache tous les paragraphes
$("p").show(); // Les réaffiche
$("p").html("Bonjour <span class='nom'>Jonathan</span> !"); // Replace leur contenu
$("p").attr("lang", "fr"); // leur ajoute l'attribut lang="fr"

// Un seul élément
console.log($("p").html()); // Affiche le contenu HTML du premier paragraphe
console.log($("p").attr("class")); // Affiche ses classes
```

## Les événements

Javascript est un langage basé sur les événements. Une fois l'intialisation
effectuée, la plupart du temps il ne se passe rien sur notre page. Pour
réagir à un événement, on demande au navigateur d'appeler une fonction (qu'on
appelle alors "callback") lorsqu'un événement se produit.

Nous allons voir l'exemple de l'événement "cliquer sur un élément HTML". En
jQuery, on déclare un callback pour cet événement ainsi :

```javascript
function maFonction() {
    console.log("J'ai cliqué !");
}
$("sélecteur").click(maFonction);
```


## Implémentation du thème sombre

Vous avez maintenant toutes les cartes en main pour permettre à l'utilisateur
de passer en thème sombre (et idéalement de repasser en thème clair) ! Pour
vous aider, voici une suggestion sur l'organisation du code :

- D'abord déclarer un tableau contenant la liste des fichiers CSS possibles.
Le premier sera celui déjà déclaré dans le code HTML (à récupérer en
Javascript), le deuxième sera `https://bootswatch.com/4/darkly/bootstrap.min.css`.
- Déclarer une variable indiquant l'indice du tableau qui correspond actuellement
au thème actif (au début 0).
- Définir une fonction permettant de changer le thème (changer l'indice du thème
actif puis changer l'attribut `href` de la balise HTML `<link>` incluant le CSS).
- Déclarer que cette fonction est un callback de l'événement "cliquer sur le
bouton de changement de thème".



# Chargement de la liste des arrêts : récupérer et traiter du JSON

Nous allons maintenant récupérer la liste des arrêts de l'API pour permettre
à l'utilisateur de choisir un arrêt grâce à la liste déroulante dans la barre
de menu.

## Boucles en Javascript

La syntaxe du while en Javascript est similaire à celle de Java, en
revanche le for est légèrement différent.

```javascript
while (condition) {
    // Instructions à répéter
}

for (let i=0; i<5; ++i) {
    console.log(i);
}

tableau = [1, 2, 3];

for (let e of tableau) {
    console.log(e);
}
```

## Les chaînes template

Nous allons prochainement construire un code HTML en Javascript.
Pour ceci, nous utiliserons un type de chaînes de caractères ajouté
il y a quelques années dans Javascript : les chaînes template. Elles
ont un comportement similaire à ce qu'on peut faire en Jinja 2.

```javascript
let a = 20;
let s = "Adrien";

let ts = `
Salut ${s} !
Il fait ${a} degrés aujourd'hui !
"Ça fait du bien !"
`;
```

On remarquera qu'il est possible d'insérer une expression Javascript
entre `${` et `}` ainsi que des retours à la ligne et les
caractères `'`, `"` à l'intérieur de la chaîne.

## Récupérer et traiter un fichier JSON

Nous allons appeler l'API de la TAN (enfin, le proxy fourni par notre
application Flask) pour récupérer la liste des arrêts de bus/tram.
Cette technique de chargement dynamique de contenu s'appelle AJAX.

JQuery fournit plusieurs fonctions pour faire de l'AJAX. L'une d'elle
permet de demander le chargement d'un fichier JSON et d'appeler un
callback pour le traiter une fois qu'il est reçu.

```javascript
$.getJSON("URL", traiterJSON);
function traiterJSON(objet) {
    // On utilise "objet" comme un objet Javascript
}
```

## Exécution d'une fonction quand le site est complètement chargé

En jQuery, il est possible de définir un callback pour l'événement "site
complètement chargé" avec les syntaxes (très minimalistes) :

```javascript
$(callback);

// ou

$(function() {
   // Contenu
})
```

## Implémentation du chargement de la liste d'arrêts

Vous pouvez maintenant compléter la partie 2 du fichier Javascript.
Pour rappel, nous souhaitons charger la liste des arrêts de la TAN
et permettre à l'utilisateur d'en choisir un dans la liste déroulante
dans la barre de menu (l'identifiant de la liste est `ligne-select`).
Voici une suggestion :

- Commencer par écrire une fonction qui fera l'appel AJAX. L'URL à
appeler ici est `/liste_arrets`.
- Écrire la fonction callback qui traitera la liste des arrêts. Cette
fonction générera une nouvelle liste d'options pour la liste déroulante,
puis insérera ce code HTML à l'intérieur de l'élément HTML d'identifiant
`ligne-select`. Pour rappel, voici à quoi ressemble la liste d'arrêts
reçue (que vous pouvez traiter comme un objet Javascript dans la fonction
de callback) :

```json
[
   ...
   {  
      "codeLieu":"CNAV",
      "libelle":"Chantiers Navals",
      "distance":null,
      "ligne":[  
         {  
            "numLigne":"1"
         }
      ]
   },
   {  
      "codeLieu":"CTRE",
      "libelle":"Chantrerie",
      "distance":null,
      "ligne":[  
         {  
            "numLigne":"C6"
         },
         {  
            "numLigne":"75"
         },
         {  
            "numLigne":"115"
         }
      ]
   },
   ...
]
```

Et voici à quoi le HTML généré devrait ressembler (la liste des lignes
entre parenthèses est optionnelle) :

```html
<option value="">Sélectionner un arrêt</option>
...
<option value="CNAV">Chantiers Navals (1)</option>
<option value="CTRE">Chantrerie (C6, 75, 115)</option>
...
```

La propriété `value` de la balise `<option>` nous permettra par la suite
de connaître le code de l'arrêt choisi par l'utilisateur, que nous devrons
fournir pour récupérer les horaires à un arrêt.

- Ajouter un callback pour l'événement de fin de chargement déclenchant le
chargement de la liste des arrêts.


# Chargement des horaires d'un arrêt : choix de liste et périodicité

Nous allons maintenant afficher la liste des prochains passages à l'arrêt
sélectionné par l'utilisateur, puis mettre cette liste à jour périodiquement.

## Événement changement de valeur d'une liste déroulante

Quand l'utilisateur change la valeur d'une liste déroulante, un événement
Javascript est émis. Il est possible d'ajouter un callback à cet événement
grâce au code jQuery suivant :

```javascript
$('sélecteur').change(callback);
```

Notez qu'il est aussi possible de définir directement la fonction au lieu de
passer une fonction définie précédemment. Il est aussi possible d'accéder à
l'élément choisi (et notamment à son attribut `value` qui nous intéresse) en
utilisant le mot clé `this`. Par exemple :

```javascript
$('sélecteur').change(function() {
    console.log(this.value);
});
```

## Callback périodique

Il est possible en Javascript de définir un callback qui sera appelé de
manière périodique grâce à la fonction suivante :

```javascript
setInterval(callback, 8000); // Toutes les 8 secondes (temps donné en ms)
```

## Paramètres par défaut d'une fonction

On peut définir en Javascript des valeurs par défaut pour les paramètre des
fonctions. Par exemple :

```javascript
function afficheNombre(n=0) {
    console.log(n);
}
afficheNombre();  // 0
afficheNombre(5); // 5
```

## Implémentation de l'affichage des horaires

Vous pouvez maintenant compléter la partie 3 du code. Vous remarquerez qu'une
fonction `directionHtml` vous est déjà fournie : elle sert à mettre en forme
les données de passages.

De plus, nous n'utiliserons pas directement le format renvoyé par l'API de la
TAN qui n'est pas pratique. Vous n'avez cependant rien à faire, la conversion
de format est déjà codée dans l'application Flask.

Vous devrez faire en sorte que quand l'utilisateur choisisse un arrêt, les
horaires qui correspondent à cet arrêt soient chargés et affichés. De plus,
les horaires devront se mettre à jour toutes les 30 secondes. Voici une
suggestion d'organisation du code :

- Commencer par initialiser une variable "arrêt courant" qui nous permettra
de savoir quels horaires charger quand nous feront la mise à jour périodique.
Par défaut, cette variable devrait valoir `""`.
- *Bonus : sauvegarder le contenu de l'élément HTML d'identifiant `lignes`
(typiquement un message de bienvenue) pour pouvoir le rafficher ultérieurement.*
- Écrire une fonction qui va faire l'appel AJAX pour charger les prochains
passages à l'arrêt courant (URL `/arret/<code>`). *Bonus : vous pouvez utiliser
un paramètre booléen pour qu'on puisse afficher un message de chargement
si l'utilisateur vient de changer de ligne, et ne pas en afficher si on fait
juste une mise à jour des horaires. De plus, on peut rafficher le message de
bienvenue précédemment sauvegardé si l'utilisateur choisit "Sélectionner un 
arrêt".*
- Écrire une fonction callback qui traitera la réponse de l'API et générera
un code HTML puis l'insérera dans l'élément HTML d'identifiant `lignes`. Les
données que vous receverez seront sous cette forme (exemple pour Chantrerie) :

```json
[
   {  
      "nom":"75",
      "type":3,
      "directions":[  
         {  
            "passages":[  
               {  
                  "no_terminus":1,
                  "temps":"3 mn"
               },
               {  
                  "no_terminus":1,
                  "temps":"32 mn"
               }
            ],
            "terminus":{  
               "Charbonneau":1
            }
         },
         {  
            "passages":[  
               {  
                  "no_terminus":1,
                  "temps":"17 mn"
               },
               {  
                  "no_terminus":1,
                  "temps":"45 mn"
               }
            ],
            "terminus":{  
               "Facultés":1
            }
         }
      ]
   },
   {  
      "nom":"C6",
      "type":3,
      "directions":[  
         {  
            "passages":[  
               {  
                  "no_terminus":1,
                  "temps":"11 mn"
               },
               {  
                  "no_terminus":1,
                  "temps":"23 mn"
               },
               {  
                  "no_terminus":1,
                  "temps":"35 mn"
               },
               {  
                  "no_terminus":1,
                  "temps":"47 mn"
               }
            ],
            "terminus":{  
               "Hermeland":1
            }
         }
      ]
   }
]
```

Vous remarquerez que les tableau `directions` contiennent une liste d'objets
indiquant les prochains horaires et terminus possibles pour une direction d'une
ligne. Ces objets seront directement traités par la fonction fournie
`directionHtml`. Le format HTML attendu est le suivant :

```html
<div class="ligne col-xl-3 col-sm-6 col-12">
    <div class="card">
        <h2 class="card-header">75</h2>
        <div class="card-body">
            <!-- Direction 1 (fourni par la fonction directionHtml) -->
            <!-- Direction 2 (fourni par la fonction directionHtml) -->
        </div>
    </div>
</div>
<div class="ligne col-xl-3 col-sm-6 col-12">
    <div class="card">
        <h2 class="card-header">C6</h2>
        <div class="card-body">
            <!-- Direction 1 (fourni par la fonction directionHtml) -->
        </div>
    </div>
</div>
```

- Définir un callback qui changera la valeur de la variable d'arrêt courant et
appellera la fonction de rechargement *(bonus : avec écran de chargement)* quand
l'utilisateur changera d'arrêt dans la liste déroulante.
- Définir un callback pour la fin de chargement de la page qui demandera
l'exécution périodique de la fonction de rechargement de l'arrêt courant.


# Correction "Horaires TAN"

Correction : [https://github.com/chardetm/horairesTAN](https://github.com/chardetm/horairesTAN)


# Surprise : Throne Mash !

Voici une application utilisant tout ce qu'on a vu, avec en plus la création d'une
API en Flask, ce qui pourrait vous être utile si vous voulez créer des pages
dynamiques dans votre projet.

Code : [https://github.com/chardetm/ThroneMash](https://github.com/chardetm/ThroneMash)
Démo : [http://thronemash.jonathanpastor.fr](http://thronemash.jonathanpastor.fr)
