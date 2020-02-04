---
title: Session 6 - Mise en forme avancée avec Bootstrap
excerpt: ""
order: 6
---

Les exemples de CSS que nous avons vus jusqu'à présent étaient très
basiques et étaient appliqués à des pages elles-mêmes très simples.
Quand la mise en forme désirée devient plus complexe, de nouveaux
problèmes se posent. Par exemple, comment rendre le site consultable
sur des écrans de différentes tailles (ordinateurs, tablettes,
téléphones) ?

Un autre problème important pour les développeurs web est le fait que
des navigateurs différents interprètent le HTML et le CSS de manières
différentes. Cela est dû à des standards incomplets ou à l'utilisation
de fonctions expérimentales. Bien que les choses se soient beaucoup
améliorées ces dernières années, beaucoup d'utilisateurs utilisent des
navigateurs anciens qui ne supportent pas les dernières évolutions des
standards du web (notamment dans les pays émergents et dans les
entreprises). Développer un site qui se comporte de la manière attendue
chez tous les utilisateurs devient alors une tâche complexe et longue.

Pour atténuer en grande partie ce problème, des frameworks tels que
Bootstrap proposent aux développeurs des composants déjà optimisés pour
un maximum de navigateurs couvrant les cas d'utilisation les plus
courants. Des thèmes sont par ailleurs proposés de sorte que notre site
soit stylisé par défaut (boutons, menus, liens, tableaux, etc.).
Nous allons voir dans cette session comment baser une application
web sur le framework Bootstrap pour fournir de manière portable un site
bien présenté et "responsive" (qui s'adapte aux différentes tailles
d'écran).

1. TOC
{:toc}


# Modèle de page HTML pour Bootstrap

Pour utiliser Bootstrap, il faut dans un premier temps inclure le fichier
CSS Bootstrap. Ceci est fait grâce à une balise `<link>` dans `<head>`.
Certains composants nécessitent d'inclure un fichier Javascript Bootstrap
ainsi que ces dépendances : JQuerry et Popper. Une bonne pratique est de
charger les fichiers Javascript non-essentiels au chargement de la page
à la fin de la balise `<body>`. On fait ceci avec trois balises
`<script>`. Enfin, pour obtenir un affichage correct sur les navigateurs
mobiles, il faut rajouter une balise `<meta>` dans `<head>` qui désactive
le comportement par défaut des navigateurs web mobile : simuler un grand
écran et permettre à l'utilisateur de zoomer. Ce comportement est actif
par défaut car une partie non-négligeable des sites internet ne sont pas
optimisés pour un affichage sur mobile.

Vous pouvez utiliser le modèle ci-dessous pour avoir une page vierge
utilisant Bootstrap :

```html
<!doctype html>
<html lang="fr">
  <head>
    <meta charset="utf-8">
    <!-- Mise à l'echelle pour les navigateurs mobiles -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- CSS de Bootstrap -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <title>Titre de la page</title>
  </head>
  <body>
    <!-- Votre contenu -->

    <!-- Fichiers Javascript nécessaires pour certains composants Bootstrap -->
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
  </body>
</html>
```

Note : si par curiosité vous voulez voir à quoi ressemblent les fichiers
CSS et Javascript de Bootstrap, vous pouvez consulter leur version
non-compressée
[ici pour CSS](https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.css)
et
[là pour Javascript](https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.js).


# Le responsive design : les media-queries

Le "responsive design" consiste à concevoir une page web de
manière à ce qu'elle s'adapte à la taille de l'écran ou de la
fenêtre de l'utilisateur en temps réel. Bootstrap fournit des
outils pour concevoir facilement un site "responsive". Pour
comprendre comment ces outils fonctionnent et comment les
compléter si besoin, nous allons voir dans cette section
comment fonctionnent les media queries.

Les media queries sont une fonctionnalité de CSS qui permet
de n'appliquer une ou plusieurs règles CSS seulement si
l'espace dédié à l'affichage de la page sur le terminal de
l'utilisateur (écran de téléphone, fenêtre de navigateur
sur ordinateur) possède certaines propriétés. La propriété
qui est la plus couramment utilisée est la largeur de
l'écran.

Considérons l'exemple interactif suivant :
[http://tiny.cc/SARfb](http://tiny.cc/SARfb)

L'affichage est correct en écran large :
![Site en écran large](/assets/img/session6/fakebook_wide.png)

Mais illisible quand l'écran est plus petit :
![Site en écran large](/assets/img/session6/fakebook_small.png)

Une solution est de, quand l'écran est trop petit,
n'afficher qu'une seule colonne par ligne dans la partie
inférieure. Pour cela, on va donner à ces colonnes une
largeur de 100% (moins 20 pixels pour des raisons de mise
en page) si l'écran est moins large que 800px :

```css
@media (max-width: 800px) {
  .paslarge {
    width: calc(100% - 20px);
  }
}
```

Pour que cela fonctionne, il faut mettre cette media
querry après la règle qui définit la largeur de .paslarge
dans le cas général, car la règle active déclarée en dernier
a priorité sur les autres en cas de conflit (si ces règles
portent sur les mêmes éléments).

Dans la version suivante de l'exemple interactif, deux media
querries font que l'affichage est à une colonne jusqu'à 600
pixels de large, deux colonnes de 600 à 1200 pixels, et
quatre colonnes au-delà de 1200 pixels :
[http://tiny.cc/SARfbr](http://tiny.cc/SARfbr)


# La grille Bootstrap

Pour simplifier le développement responsive, Bootstrap
propose le principe de conteneur contenant des lignes à
12 colonnes. Pour chaque élément dans le conteneur, on
indique combien de colonnes il prend en fonction de la
taille de l'écran. Si la somme des largeurs dans une
ligne fait plus de 12, les éléments qui dépassent sont
automatiquement mis sur une nouvelle ligne.

![La grille Bootstrap](/assets/img/session6/bootstrap_grid.png)

Ainsi, pour reproduire l'exemple précédent avec Bootstrap,
il suffit de :
- mettre nos éléments dans un conteneur
- mettre le panneau de bienvenue dans une ligne et les
quatre autres dans une deuxième
- indiquer que le premier panneau prend 12 colonnes
- indiquer que sur les petits écrans les autres panneaux
prennent 12 colonnes (toute la largeur), sur les écrans
moyens 6 colonnes (la moitié) et sur les grands écrans 3
colonnes (le quart).

Pour cela, nous allons utiliser les tailles d'écran
prédéfinies dans Bootstrap :
* xs : ≥ 0px
* sm : ≥ 576px
* md : ≥ 768px
* lg : ≥ 992px
* xl : ≥ 1200px

Pour indiquer qu'un élément doit prendre 3 colonnes à
partir de 1200 pixels de large, on lui donne la classe
"col-xl-3". Pour indiquer qu'il doit prendre 6 colonnes
à partir de 576 pixels de large, on lui donne la classe
"col-sm-6". Enfin, pour indiquer que par défaut (donc
dans ce cas pour les écrans de moins de 576 pixels de
large) il doit prendre 12 colonnes, on lui donne la
classe "col-12". Nos éléments seront donc déclarés
comme ceci :

```html
<div class="container-fluid"> <!-- Conteneur pleine largeur -->
  <div class="row"> <!-- Première ligne -->
    <div class="col-12">
      Premier panneau
    </div>
  </div>
  <div class="row"> <!-- Deuxième ligne -->
    <div class="col-xl-3 col-sm-6 col-12">
      Petit panneau 1
    </div>
    <div class="col-xl-3 col-sm-6 col-12">
      Petit panneau 2
    </div>
    <div class="col-xl-3 col-sm-6 col-12">
      Petit panneau 3
    </div>
    <div class="col-xl-3 col-sm-6 col-12">
      Petit panneau 4
    </div>
  </div>
</div>
```

Nous n'avons donc plus besoin de CSS pour gérer la
taille des éléments, donc plus besoin non plus de
media querries !

Exemple interactif :
[http://tiny.cc/SARfbbs](http://tiny.cc/SARfbbs)

La grille Bootstrap permet d'autres choses que nous
ne couvrons pas ici, telles que l'alignement vertical
des éléments dans une ligne (si un élément de la ligne
est plus haut que les autres), l'alignement et
l'espacement horizontal des éléments dans une ligne
(s'ils ne remplissent pas les 12 colonnes),
l'imbriquement de grilles etc. Pour en savoir plus :
[https://getbootstrap.com/docs/4.0/layout/grid/](https://getbootstrap.com/docs/4.0/layout/grid/)


# Composants Bootstrap

## Classes visuelles

Bootstrap fournit un ensemble de classes à donner
à des éléments HTML existants pour leur donner une
apparence bien plus élégante que leur version par
défaut. Utilisées sur tous les éléments d'une page,
elles permettent aussi d'obtenir une bonne cohérence
visuelle très simplement.

Un exemple est la classe "table" à donner à une
balise `<table>`. D'autres classes sont disponibles
pour personnaliser très facilement le rendu d'un
tableau. Pour en savoir plus :
[https://getbootstrap.com/docs/4.0/content/tables/](https://getbootstrap.com/docs/4.0/content/tables/)

## Composants personnalisés

Bootstrap propose un grand catalogue de composants
qu'il est très facile d'utiliser. Ces composants
sont soit de grosses améliorations de ceux fournis
par HTML (boutons, formulaires, ...) soit des
composants qui ne sont pas proposés par HTML et
qu'il faudrait donc coder à la main (cartes,
badges, diaporamas, barres de navigation, menus,
pages, ...).

Deux types de composants qui vous seront sans
doute utiles dans votre projet sont les boutons
et les barres de navigation. Vous avez toutes
les notions nécessaires pour comprendre leur
documentation, que vous utiliserez pour améliorer
la mise en forme de vos templates. N'hésitez pas
à jeter un œil aux autres composants (menu à
gauche sur la documentation Bootstrap), ces
composants sont très faciles à utiliser et les
efforts pourraient être récompensés lors de
l'évaluation ! :-)

### Exemple 1 : boutons Bootstrap

Documentation des boutons Bootstrap :
[https://getbootstrap.com/docs/4.0/components/buttons/](https://getbootstrap.com/docs/4.0/components/buttons/)

![Boutons Bootstrap](/assets/img/session6/bootstrap_buttons.png)

```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>

<button type="button" class="btn btn-link">Link</button>
```

### Exemple 2 : barres de navigation

Documentation des barres de navigation :
[https://getbootstrap.com/docs/4.0/components/navbar/](https://getbootstrap.com/docs/4.0/components/navbar/)

![Barre de navigation Bootstrap](/assets/img/session6/bootstrap_navbar.png)

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <a class="navbar-brand" href="#">Navbar</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarColor01" aria-controls="navbarColor01" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarColor01">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Features</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Pricing</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">About</a>
      </li>
    </ul>
    <form class="form-inline">
      <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" />
      <button class="btn btn-outline-info my-2 my-sm-0" type="submit">Search</button>
    </form>
  </div>
</nav>
```
