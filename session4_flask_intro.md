---
title: Session 4 - Développer une application Flask simple
excerpt: ""
order: 4
---

1. TOC
{:toc}


Avant de démarrer cette session, assurez-vous de bien avoir un
environnement de développement Flask fonctionnel. Si ce n'est pas le
cas, veuillez suivre les instructions de la [session 3](/session3_install.html)
avant de poursuivre avec les instructions ci-dessous.


# Une première application web simple, avec Flask

Nous allons voir dans cette Section comment afficher un message dans le navigateur de l'utilisateur. 

Tout d'abord, créer un nouveau projet Flask qui aura
pour nom "FlaskTP1" en suivant les instructions de la [session
précédente](http://localhost:4000/session3_install.html#v%C3%A9rification-de-linstallation).

Nous allons maintenant coder une vue qui affichera le texte
"Helloworld". Pour cela, éditez le fichier `app.py` généré par PyCharm
de manière à ce qu'il contienne le code suivant:

```python
@app.route('/greetings')
def some_function():
    return "Helloworld!"
```

Lancer l'application Flask, et accéder à cette vue via l'URL [http://127.0.0.1:5000](http://127.0.0.1:5000):

![capture d'écran montrant le resultation d'un helloworld avec Flask](/assets/img/session1/screen1.png)

Nous pouvons faire les observations suivantes:

* L'URL à entrer dans le navigateur est définie par la valeur passée à `@app.route`
* Le résultat de l'exécution de la fonction est envoyé au navigateur
* **Le nom donné à la fonction Python n'a pas d'importance**



# Passer de l'information dans l'URL

Il est possible de récupérer des informations contenues dans l'URL que
l'utilisateur accède. Le code suivant définit une fonction qui prend 3
paramètres, et affiche un calcul:

```python
@app.route("/sum/<lang>/<int:a>/<int:b>")
def compute_sum(lang, a, b):
    c = a + b
    if lang == "fr":
        return "La somme de %d et %d est %d" % (a, b, c)
    else:
        return "The sum of %d and %d is %d" % (a, b, c)
```

Quand on accède à l'URL [http://127.0.0.1:5000/sum/en/3/5](http://127.0.0.1:5000/sum/en/3/5), on obtient le
résultat suivant:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session1/screen5.png)


# Vers des vues structurées avec les templates

Dans cette section nous verrons comment construire une réponse
structurée. Dans un premier temps nous retournerons un texte généré en
Python et transmis à Flask. Dans un second temps, nous verrons comment
écrire la même chose en utilisant le moteur de template Jinja2.


## Une vue construite naïvement en Python

Prenons l'exemple suivant:

```python
import flask # mettre cette ligne en debut de fichier

@app.route("/complex_view")
def complex_view():
    # variables
    colors = ["red", "blue", "green", "purple", "dark", "white"]
    boolean_value = False
    msg_if_boolean_value = "[boolean_value is true]"
    msg_if_not_boolean_value = "[boolean_value is false]"

    # construction de la reponse
    result = ""

    for color in colors:
        result += "* "
        result += color
        result += "\n"

    if boolean_value:
        result += msg_if_boolean_value
    else:
        result += msg_if_not_boolean_value

    # envoie de la reponse sous forme textuelle
    return flask.Response(result,
                          mimetype="text")
```

En visitant l'URL
[http://127.0.0.1:5000/complex_view](http://127.0.0.1:5000/complex_view),
vous obtiendez ce resultat: ![capture d'écran montrant le result du
premier programme avec une vue
complexe](/assets/img/session4/complex_view.png)

La fonction `complex_view` retourne une réponse textuelle construite
avec deux structures de contrôle : un `for` et un `if`. Bien qu'étant
fonctionnelle, cette fonction deviendrait difficilement lisible si le
texte à ajouter devant une couleur était plus long.

__De plus, en procédant de la sorte, le code la fonction Python
contient la logique de la présentation, ce qui est en contradiction
avec la séparation MVC.__

## La même vue construite avec une template Jinja

**L'objectif sera maintenant de coder l'exemple précédent, en
s'appuyant sur des templates Jinja2 en prenant en compte des variables
Python la génération de la réponse. Nous verrons notamment comment
faire une boucle avec l'instruction `for` et comment faire une
structure conditionnelle avec l'instruction `if/else`**

Pour cela, nous allons:

- 1) créer une nouvelle fonction `complex_view_template` qui contient le code suivant:

```python
@app.route("/complex_view_template")
def complex_view_template():
    colors = ["red", "blue", "green", "purple", "dark", "white"]
    boolean_value = False
    msg_if_boolean_value = "[boolean_value is true]"
    msg_if_not_boolean_value = "[boolean_value is false]"

    # Bien penser a mettre 'import flask' en debut de fichier
    return flask.Response(flask.render_template("complex_view.jinja2",
                                 colors=colors,
                                 boolean_value=boolean_value,
                                 msg_if_boolean_value=msg_if_boolean_value,
                                 msg_if_not_boolean_value=msg_if_not_boolean_value),
                          mimetype="text")
```

- 2) Créer un fichier `templates/complex_view.jinja2` de manière à ce qu'elle affiche les variables passées à la fonction `render_template`:

{% raw %}
```jinja
{% for color in colors %}
    {{ color }}
{% endfor %}

{% if boolean_value_arg %}
    {{ msg_if_boolean_value_arg }}
{% else %}
    {{ msg_if_not_boolean_value_arg }}
{% endif %}
```
{% endraw %}

En visitant la page [http://127.0.0.1:5000/complex_view_template](http://127.0.0.1:5000/complex_view_template), vous obtiendez un résultat proche de:

![capture d'écran montrant le resultat du premier programme utilisant les templates](/assets/img/session4/complex_view_template.png)


On peut faire les observations suivantes:
- les valeurs des variables Python sont bien prises en compte pour
  former la réponse HTML : la réponse HTML sera adaptée au contenu des
  variables Python.
- De plus des structures de contrôle telles que les
  `if/else` et les `for/while` permettent de structure le code Jinja2.
- **Des espaces sont ajoutés entre les lignes**

**(Bonus)**

Bien que les espaces en trop ne posant pas de soucis lors de la
génération de code HTML, il pourrait en être autrement lors de la
génération d'une réponse qui serait sensible aux espaces. Dans un tel
cas, il est possible d'avoir un plus grand contrôle sur la gestion des
espaces (c.f. [ce
lien](http://jinja.pocoo.org/docs/2.10/templates/#whitespace-control)).

Le code de template suivant permet d'avoir un résultat identique à
celui de la fonction en python "pur':

{% raw %}
```jinja
{% for color in colors -%}
  * {{ color }}
{% endfor %}

{%- if boolean_value -%}
    {{ msg_if_boolean_value }}
{%- else -%}
    {{ msg_if_not_boolean_value }}
{% endif %}
```
{% endraw %}

![capture d'écran montrant le resultat du premier programme utilisant les templates et gestion des espaces](/assets/img/session4/complex_view_template_1.png)

# Envoyer du HTML à l'utilisateur

Dans cette section nous allons voir comment renvoyer une réponse HTML
à l'utilisateur en utilisant les templates Jinja.


Tout d'abord, récupérer une archive de code
[session_4_engineer_view.zip](https://github.com/badock/ue_web_2020_example/archive/session_4_engineer_view.zip)
contenant un projet qui servira de base pour cet exercice. 

Ce TP contient une base de données pour stocker les données. Entre
deux requêtes, les variables locales à une fonction python ne sont pas
partagées et les variables globales seront perdues quand le serveur Flask
est redémarré. Dans le cas présent, la base de données est configurée
pour s'initialiser toute seule.

**Les bases de données seront introduites dans une future session.**

Le projet contient aussi une classe `Engineer` persistante:

```python
class Engineer(db.Model):
    id = <int>
    username = <string>
    email = <string>
    site = <string>

    def __repr__(self):
        return '<Engineer {}>'.format(self.username)
```

Cette classe hérite de `db.Model`, ce qui permet de stocker les
instances de cette classe dans la base de données évoquée
précédemment. Au démarrage du serveur Flask, les ingénieurs
précédemment ajoutés seront présents.

Ce projet contient aussi un fichier app.py qui s'occupe d'initialiser la base de données et qui propose:
  - une fonction `get_all_engineers` qui retourne une liste d'ingénieurs
  - une fonction `get_engineer_by_id` qui retourne un ingénieur en fonction de son identifiant
  - une fonction `get_engineers_in_site` qui retourne les ingénieurs présents sur un site passé en paramètre
  - une vue `index` affichant les ingénieurs présents dans la base de données
  - une vue `show_engineer` qu'il faudra coder dans cet exercice


__Nous allons maintenant coder une vue qui affichera la description d'un
ingénieur en prenant en entrée son identifiant__. Pour cela, nous
allons:

* 1) créer un fichier `show_engineer.html.jinja2` dans le dossier templates
   (en violet dans PyCharm)
* 2) y mettre le contenu suivant:

{% raw %}
```html
<html>
    <head>
        <title>Description de {{ engineer.username }}</title>
    </head>
    <body>
        <p>Information about "{{ engineer.username }}"</p>
    
        <dl>
            <dt>id</dt>
            <dd>{{ engineer.id }}</dd>
        
            <dt>username</dt>
            <dd>{{ engineer.username }}</dd>
        
            <dt>email</dt>
            <dd>{{ engineer.email }}</dd>
        
            <dt>site</dt>
            <dd>{{ engineer.site }}</dd>
        </dl>
    </body>
</html>
```
{% endraw %}

- 3) créer une nouvelle fonction python dans le fichier `app.py`, qui reprendrait le code suivant:

```python
@app.route('/engineer/id/<int:engineer_id>')
def show_engineer_by_id(engineer_id):
    engineer = get_engineer_by_id(engineer_id)

    # Bien penser a mettre 'import flask' en debut de fichier
    return flask.render_template("show_engineer.html.jinja2",
                                 engineer=engineer)
```

En visitant la page [http://127.0.0.1:5000/engineer/id/1](http://127.0.0.1:5000/engineer/id/1), le résultat suivant devrait être affiché:
![description d'un ingénieur](/assets/img/session4/engineer_description.png)


# Exercice

Pour la prochaine séance, nous vous demandons de codez une vue qui:
- prendra en paramètre le nom d'un site (brest, nantes, ...)
- affichera les ingénieurs affiliés à ce site

Pour cela vous pourrez:
- utiliser la fonction `get_engineers_in_site` qui a un site retourne les ingénieurs qui y sont affiliés
- créer une template jinja `display_engineers_by_site.html.jinja2`
- vous inspirer des templates vues en cours en utilisant un `for` et {% raw %}`{{ engineer.X }}`{% endraw %}
- Suggestion: vous pouvez afficher les ingénieurs sous forme de liste HTML en utilisant la structure:

```html
<ul>
    <li>ingenieur A</li>
    <li>ingenieur B</li>
    <li>ingenieur C</li>
</ul>
```
