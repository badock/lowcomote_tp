---
title: Session 2 - Le modèle serveur-client par l'exemple
excerpt: ""
order: 2
---


Une application web fonctionne sur deux supports : le serveur, qui est
une machine connectée au web appartenant au fournisseur de l'application,
et le client, qui est en général le navigateur internet de l'utilisateur.
Une application web très utilisée de nos jours est le réseau social
Facebook, que nous prendrons comme exemple pour illustrer le fonctionnement
d'une telle application.

1. TOC
{:toc}

# Affichage d'une page

Considérons la page Facebook de l'IMT Atlantique :
[https://www.facebook.com/IMTAtlantique/](https://www.facebook.com/IMTAtlantique/).
On peut en tirer trois informations :
* Le protocole à utiliser : HTTP sécurisé (`https`)
* Le serveur à contacter : `www.facebook.com`
* La page à demander au serveur (appelée 'route') : `/IMTAtlantique/`

## Côté client

![affichage d'une page côté client](/assets/img/session1/client_get.png)

Quand l'utilisateur demande au navigateur internet de voir cette page (en
tapant son adresse ou en cliquant sur un lien y menant), ce dernier envoie
une requête `GET '/IMTAtlantique/` au serveur de Facebook. Le serveur
construit et renvoie au navigateur un fichier (au format HTML, que nous
étudierons plus en détail par la suite) qui contient le contenu de la page
ainsi que des liens vers les ressources complémentaires utiles à l'affichage
de la page. À ce stade, si le navigateur affichait la page à l'utilisateur,
elle ressemblerait à ça :

![rendu de la page sans CSS ni Javascript](/assets/img/session1/nocss.png)

Les ressources complémentaires que le navigateur récupère ensuite (grâce à
des requêtes GET similaires à la requête de la page) comprennent : les images,
vidéos, sons, etc., mais surtout les feuilles de style CSS et les scripts
Javascript. On obtient alors le rendu suivant :

![rendu de la page avec CSS et Javascript](/assets/img/session1/withcss.png)

## Côté serveur

![affichage d'une page côté serveur](/assets/img/session1/server_get.png)

Dans la sous-section précédente, nous avons vu que le serveur construisait une
page qui était envoyée au navigateur web de l'utilisateur (le client). Nous
allons maintenant nous intéresser à comment cette page est construite. Quand
l'application serveur reçoit la requête `GET '/IMTAtlantique/'`, elle analyse la
demande et appelle une fonction permettant de générer la page Facebook de l'IMT
Atlantique.

Cette génération se fait en trois étapes :
1. Récupération du template (ici celui d'une page Facebook)
2. Récupération des informations à insérer (ici les informations de la page IMT Atlantique et sa liste de publications)
3. Remplissage du template avec les informations

Un template est un modèle, une sorte de texte à trous intelligent, qui permet
de ne pas réécrire les éléments communs à plusieurs pages web. Par exemple,
la barre supérieure est commune au site Facebook entier, tandis que le bandeau
de page à gauche est commun à toutes les pages Facebook. Voici une représentation
visuelle du template de page Facebook :

![représentation visuelle du template de page Facebook](/assets/img/session1/template.png)

Les "trous" (indiqués sur l'image par des noms entre accolades pour le texte et des
fonds verts pour les images) sont ensuite remplis en récupérant dans un premier temps les
informations spécifiques à la page demandée dans une base de données, puis en instanciant
le template avec les données récupérées. C'est l'instance obtenue qui est renvoyée au
client.


# Envoi d'un formulaire

Jusqu'à maintenant, le client ne faisait qu'afficher du contenu fourni par le serveur.
Considérons un cas où le client envoie des informations au serveur que ce dernier doit
traiter : l'envoi d'un commentaire sur une publication. Voici à quoi ce formulaire simple
ressemble :

![formulaire d'envoi d'un commentaire](/assets/img/session1/emptycomment.png)

À l'envoi, ce formulaire contient trois informations :

* Le texte du commentaire (entré par l'utilisateur)
* L'identifiant de la publication commentée (dans le code HTML mais masqué par le navigateur)
* La route à laquelle envoyer les informations (idem)

## Côté client

![affichage d'une page côté client](/assets/img/session1/client_post.png)

Quand l'utilisateur demande la publication du commentaire, le navigateur envoie une
requête `POST '/api/graphql/'` contenant le message entré par l'utilisateur et le
numéro de la publication. Il reçoit ensuite une réponse du serveur (`200` si tout
s'est bien passé), et un des scripts Javascript de la page Facebook va alors mettre
à jour la page et afficher le nouveau commentaire posté.

## Côté serveur

![affichage d'une page côté client](/assets/img/session1/server_post.png)

À la réception de la requête `POST`, l'application serveur analyse la demande et appelle
une fonction dont le but est de poster le commentaire d'une publication. Cette dernière
communique alors avec la base de données contenant les commentaires de publications et
demande l'ajout du commentaire. Si tout s'est bien produit, l'application serveur envoie
au client le code 200. Dans le cas contraire, un autre code est renvoyé.
