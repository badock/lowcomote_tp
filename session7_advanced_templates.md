---
title: Session 7 - Templates avancées et ressources statiques
excerpt: ""
order: 7
---

1. TOC
{:toc}

Pour cette session nous utiliserons un projet déjà existant. Veuillez
récupérer ZIP l'archive accessible à l'URL suivante [tp_templates_start.zip](https://github.com/badock/ue_web_2020_example/archive/tp_templates_start.zip).

Ce projet contient 3 vues:
- view_a
- view_b
- view_c

qui ont le même code et font appel à des templates dont le code est très similaire,
et donc affichent toutes une page web similaire, dont seule une partie du
code HTML change. Jetez un oeil à ces fonctions et à ces templates.


Il y a clairement duplications de code, ce qui contrevient au principe
[`Do not repeat
yourself`](https://fr.wikipedia.org/wiki/Ne_vous_r%C3%A9p%C3%A9tez_pas)
recherché lors du développement de logiciels.

Dans cette session, nous verrons comment utiliser des fonctions
avancées du moteur de templates "Jinja2" pour limiter les répétitions
de code.

# Templates Jinja avancées

Pour cet exemple, nous utiliserons du code CSS intégré directement
dans la page entre deux balises `<style>`. Cette pratique est
généralement découragée, mais simplifiera la compréhension dans ce
cas.

## Héritage de templates

### Introduction

On peut avec Jinja définir des blocs à l'intérieur d'une template avec
les instructions `block`:

{% raw %}
```jinja
{% block nom_du_bloc %}
ancienne valeur
{% endblock %}
```
{% endraw %}

On peut ensuite de définir une template "filles", héritant d'une template mère, et qui modifiera la valeur d'un des blocs de la template mère. Cela se fait en:
1. déclarant une nouvelle template (la template fille)
2. indiquant que la template fille étend la template mère avec l'instruction `extends`:{% raw %}
```jinja
{% extends "template_mere.jinja2" %}
```
{% endraw %}
3. en redéfinissant le bloc dans la template fille: {% raw %}
```jinja
{% block nom_du_bloc %}
nouvelle valeur
{% endblock %}
```
{% endraw %}

### Exercice

Définir un nouveau fichier template `templates/layout.html.jinja2`, et
y mettre le code suivant:

{% raw %}
```jinja
<html>
    <head>
        <title>{{ title }}</title>
        <!-- <style> est une balise qui permet d'inclure du code CSS dans une page web, 
        sans définir un fichier externe de style CSS -->
        <style>
            .active {
                color: red;
                font-weight: bold;
            }
        </style>
    </head>
    <body>
    <header>
        <a href="/view/a" class="active">Template A</a> |
        <a href="/view/b">Template B</a> |
        <a href="/view/c">Template C</a>
        <hr>
    </header>
   {% block body %}
   {% endblock %}
    <footer>
        <hr>
        Flask SAR - 2019
    </footer>
    </body>
</html>
```
{% endraw %}

et modifier les templates:
- templates/template_a.html.jinja2
- templates/template_b.html.jinja2
- templates/template_c.html.jinja2

pour qu'elles étendent la template `layout.html.jinja2` et adapter le message affiché au fichier de template:
{% raw %}
```jinja
{% extends "layout.html.jinja2" %}

{% block body %}
    <p>Template A</p>
{% endblock %}
```
{% endraw %}

Nous avons factorisé le code des templates. Cependant, l'affichage de
la page active dans la barre du haut n'est pas bon : la template A est
marquée sélectionnée, même si nous naviguons sur les pages des
templates B et C. Nous verrons dans la section suivante comment gérer
cet affichage avec des variables Jinja.

## Manipulation de variables

Nous allons maintenant voir comment ajouter des variables jinja2 dans les templates filles, afin d'indiquer à la template "racine" quelle est la page  active ainsi que le nom de la page. Ces variables seront prises en compte dans le code de la template `layout.html.jinja2`.

Premièrement, modifier les templates:
- templates/template_a.html.jinja2
- templates/template_b.html.jinja2
- templates/template_c.html.jinja2

pour qu'elles aient la structure suivante:
{% raw %}
```jinja
{% extends "layout.html.jinja2" %}
{% set title = "template A" %}
{% set active_page = "view_a" %}

{% block body %}
    <p>Template A</p>
{% endblock %}
```
{% endraw %}

Nous allons maintenant modifier la template `layout.html.jinja2`, afin
qu'elle prenne en compte les précédentes variables:

{% raw %}
```jinja
<html>
    <head>
        <title>{{ title }}</title>
        <style>
            .active {
                font-weight: bold;
                color: red;
            }
        </style>
    </head>
    <body>
    <header>
        <a href="{{ url_for("view_a") }}" {% if active_page == "view_a" %}class="active"{% endif %}>Template A</a> |
        <a href="{{ url_for("view_b") }}" {% if active_page == "view_b" %}class="active"{% endif %}>Template B</a> |
        <a href="{{ url_for("view_c") }}" {% if active_page == "view_c" %}class="active"{% endif %}>Template C</a>
        <hr>
    </header>
    {% block body %}
    {% endblock %}
    <footer>
        <hr>
        Flask SAR - 2019
    </footer>
    </body>
</html>
```
{% endraw %}

En allant sur l'URL
[http://127.0.0.1:5000/view/b](http://127.0.0.1:5000/view/b), vous
devriez obtenir le résultat suivant:
![/assets/img/session7/template_b.png](/assets/img/session7/template_b.png)


## Macro

Jinja fournit un mécanisme de macros qui permet de factoriser
certaines instructions sous la forme d'une fonction. Leur intérêt est
de limiter les répétitions de code.

Dans l'exemple précédent, nous avions ce code:
{% raw %}
```jinja
<a href="{{ url_for("view_a") }}" {% if active_page == "view_a" %}class="active"{% endif %}>Template A</a> |
<a href="{{ url_for("view_b") }}" {% if active_page == "view_b" %}class="active"{% endif %}>Template B</a> |
<a href="{{ url_for("view_c") }}" {% if active_page == "view_c" %}class="active"{% endif %}>Template C</a>
```
{% endraw %}

qui posait les problèmes suivants:
- répétition trois fois du même code
- la structure de contrôle `if` est mise sur une seule ligne, afin de
  rendre le code plus concis, mais au final l'ensemble est moins
  lisible.

La définition d'une macro dans le fichier `layout.html.jinja2` qui
prend deux paramètres. Ces paramètres sont réutilisables ensuite dans
le corps de la macro:
{% raw %}
```jinja
{% macro set_active_if(variable, name) %}
    {% if variable == name %}class="active"{% endif %}
{% endmacro %}
```
{% endraw %}

La macro étant définie, elle est utilisable en l'appelant comme une fonction pyth:
{% raw %}
```jinja
<a href="{{ url_for("view_a") }}" {{ set_active_if(active_page, "view_a") }}>Template A</a> |
<a href="{{ url_for("view_b") }}" {{ set_active_if(active_page, "view_b") }}>Template B</a> |
<a href="{{ url_for("view_c") }}" {{ set_active_if(active_page, "view_c") }}>Template C</a>
```
{% endraw %}

Le code est maintenant plus concis, et la condition `if` est plus
lisible. Vous trouverez plus d'informations sur les macros avec le
lien suivant
[http://jinja.pocoo.org/docs/2.10/templates/#macros](http://jinja.pocoo.org/docs/2.10/templates/#macros).

# Ajouter des fichiers CSS et Javascript avec Jinja

Les fichiers CSS et Javascript sont généralement des fichiers
statiques, c'est-à-dire que leur contenu ne change pas en fonction des
requêtes des utilisateurs.

Avec Flask, il faut mettre les fichiers CSS et Javascript dans le
dossier `static`.  Les fichiers présents dans le dossier `static` sont
publics, et ont chacun une URL.

Il est possible de récupérer l'URL d'un fichier statique avec la
fonction `url_for` comme dans l'exemple suivant:
```python
from flask import url_for # a mettre en debut de fichier

fichier_css_url = url_for("static", filename="css/fichier.css")
print(fichier_css_url)
```

## Sous dossiers de ressources

Créer trois dossiers `css`, `js` et `img` dans le dossier `static`
d'un projet Flask.

Créer le fichier `static/css/style.css` avec le code css suivant:
```css
.logo {
    width: 400px;
}
```

Créer le fichier `static/js/script.js` avec le code suivant:
```js
(function update_caption() {
    let caption_text = "logo IMT Atlantique";
    document.getElementById("caption_logo").innerText = caption_text;
})();
```

Ensuite, faire les actions suivantes:
1. aller à cette page
   [https://www.imt-atlantique.fr/fr/l-ecole/logotypes-imt-atlantique](https://www.imt-atlantique.fr/fr/l-ecole/logotypes-imt-atlantique)
2. télécharger un des logos, le mettre dans le dossier `static/img/`
   et le renommer `logo.jpg` (adapter l'extension de fichier à celle
   du logo téléchargé

Maintenant que nous avons préparé les resources statiques pour cet
exercice, créez une template jinja
`templates/static_resources.html.jinja2` qui contient le code suivant:

{% raw %}
```jinja
<html>
    <head>
        <title>Static resources with Flask</title>
        <link rel="stylesheet" href="{{ url_for("static", filename="css/style.css") }}"/>
    </head>
    <body>
        <img class="logo" src="{{ url_for("static", filename="img/logo.jpg") }}"/>
        <span id="caption_logo"></span>
    </body>
    <script src="{{ url_for("static", filename="js/script.js") }}"></script>
</html>
```
{% endraw %}

et appeler cette template depuis une nouvelle fonction Python:

```python
@app.route('/static_resources_view')
def static_resources_view():
    return render_template("static_resources.html.jinja2")
```

En allant sur le lien
[http://127.0.0.1:5000/static_resources_view](http://127.0.0.1:5000/static_resources_view),
vous devriez obtenir le résultat suivant:

![/assets/img/session7/static_resources.png](/assets/img/session7/static_resources.png)

En affichant le code source de la page, on peut constater que le texte
sous le logo de l'IMT n'apparait pas dans le code HTML:
![/assets/img/session7/static_resources_source.png](/assets/img/session7/static_resources_source.png)

En effet, nous l'avons fait apparaitre en modifiant le
[DOM](https://fr.wikipedia.org/wiki/Document_Object_Model) de la page
Web avec Javascript. Nous verrons dans une session future comment
faire des modifications avancées avec Javascript.

# Correction

Vous pouvez récupérer une archive ZIP [tp_templates_correction.zip](https://github.com/badock/ue_web_2020_example/archive/tp_templates_correction.zip) contenant la correction de cette séance.
