---
<!-- title: Modèle de données pour le projet -->
excerpt: ""
order: 11
---

1. TOC
{:toc}

# Introduction

Pour vous aider à avancer dans le projet, nous vous fournissons un modèle de données que vous trouverez dans l'archive [projet_demarrage.zip](https://github.com/badock/ue_web_2020_example/archive/projet_demarrage.zip).

Cette archive contient:
- Sept classes modèles représentant le SI pour l'application d'affectations de missions dans une SSII
- un script qui initialise des données de tests
- Trois vues flask permettant d'illustrer l'utilisation du modèle

# Base de données

Le modèle de données correspond à l'organisation suivante:

![diagramme de classe de la base de données fournies](/assets/img/projet_demarrage/mcd.png)

La base de données répond aux contraintes suivantes
- Des ingénieurs ont validé des compétences en passant des certifications
- Des missions ont des besoins en compétences
- Des ingénieurs peuvent se positionner sur une mission en émettant des souhaits
- Des ingénieurs participent à des missions. Leurs participations ont une date de début et de fin
- Un ingénieur peut être responsable d'une mission (dans ce cas, il devient l'ingénieur d'affaires ou responsable commercial de la mission). 

Quand vous lancez le projet Flask, la base de données est automatiquement créée, et initialisée avec des données.

# Initialisation des données

Des données sont créées par le script `database/database.py` qui est
appelé automatiquement au démarrage du projet Flask: ce script crée
des ingénieurs, des missions et des compétences, et associe des
compétences aux ingénieurs, affecte des ingénieurs aux missions.

À la fin de l'exécution de ce script, un fichier
`database/database.db` est créé. Si au lancement du script le fichier
`database/database.db` existe déjà, le script ne crée pas de nouvelles
données.

__ Pour forcer la création de nouvelles données, il faut supprimer le
fichier `database/database.db`. Attention, cela aura pour effet de supprimer
toutes les données existantes.__

Si vous changez le modèle de données (ajout de classes, modification
des attributs, ...), il faudra:
- adapter les instructions du fichier `database/database.py`
- supprimer le fichier `database/database.db` pour qu'une nouvelle
  version de la base de données soit créée.
