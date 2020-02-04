---
title: Session 3 - Démarrer avec Flask et Python
excerpt: ""
order: 3
---

1. TOC
{:toc}


Dans le cas où vous ne disposeriez pas déjà d'un environnement de
développement Python, nous en installerons un dans cette session en utilisant le logiciel
[*miniconda*](https://docs.conda.io/en/latest/miniconda.html) pour
configurer rapidement un tel environnement. Si vous disposez déjà d'un
environnement Python fonctionnel, vous pouvez sauter cette étape.


# Installation d'un environnement Python avec Miniconda et PyCharm

## Installation de Miniconda

Avec votre navigateur, visitez [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html) pour télécharger le programme d'installation qui correspond à votre configuration. Faites attention à choisir __Python 3.7__.

Il faut exécuter le programme d'installation sur votre machine. Au cours de l'installation, utilisez les paramètres par défaut. **Il est possible que le programme d'installation vous demande d'entrer un mot de passe administrateur. Dans ce cas, entrez le mot de passe de votre compte.**

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen2.png)

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen4.png)

Vous pouvez quitter le programme d'installation : Miniconda est maintenant installé sur l'ordinateur. 

**Si l'installation n'a pas fonctionné correctement ou des messages d'erreur sont apparus, parlez-en à un des enseignants afin de ne pas prendre de retard sur la session.**

## Installation et configuration de PyCharm

Avec votre navigateur, visitez [https://www.jetbrains.com/pycharm/download](https://www.jetbrains.com/pycharm/download) pour télécharger le programme d'installation de "PyCharm" qui correspond à votre configuration.

Bien que la version *PyCharm Community* puisse être suffisante pour ce module, **nous vous conseillons vivement** de faire un compte sur le site [https://www.jetbrains.com/student/](https://www.jetbrains.com/student/) avec votre adresse email "imt-atlantique.net" et de récupérer ainsi gratuitement la version *PyCharm Professional*, cette dernière incluant un plug-in qui facilite beaucoup le développement d'un projet Flask.

Démarrez le programme d'installation récupéré sur le site [https://www.jetbrains.com/pycharm/download](https://www.jetbrains.com/pycharm/download), et continuez avec une installation utilisant les paramètres par défaut.

**Si l'installation n'a pas fonctionné correctement ou des messages d'erreur sont apparus, parlez-en à un des enseignants afin de ne pas prendre de retard sur la session.**

Une fois l'installation terminée avec succès, lancez "PyCharm". Vous devriez voir apparaitre un écran d'accueil qui ressemble à cela:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen1.png)

## Vérification de l'installation

Pour vérifier que l'installation de "PyCharm" et "Miniconda" est fonctionnelle, nous allons créer un projet basique. Sur l'écran d'accueil de "PyCharm", cliquez sur *create new project*:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen13.png)

Un écran vous demandant des renseignements sur le projet apparaitra:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen9.png)

Appliquez les actions suivantes:
1. Sélectionnez **Flask** comme type de projet
2. Entrez un nom de projet (*FlaskExample*)
3. Si nécessaire, dépliez le menu **Project Interpreter**
4. Sélectionnez **Conda**
5. Validez la création du projet

PyCharm va configurer le nouveau projet et analyser votre installation
de Python, **ce qui peut prendre quelques minutes**. Au final, une fenêtre
affichant le code du projet exemple apparaitra:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen14.png)

Nous allons lancer le projet afin de voir si **Python** et **Flask** sont bien installés. Pour cela, cliquez sur l'icône en forme d'insecte en haut à droite de la fenêtre PyCharm:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen10.png)

Une console devrait apparaitre en bas de votre fenêtre PyCharm:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen11.png)


Avec votre navigateur web, tapez l'[URL](https://fr.wikipedia.org/wiki/Uniform_Resource_Locator) indiquée dans la console : [http://127.0.0.1:5000](http://127.0.0.1:5000). Un texte devrait apparaitre dans le navigateur:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen8.png)

Si ce que vous obtenez correspond à la capture d'écran ci-dessus,
alors votre environnement de développement est correctement
configuré. **Si ce n'est pas le cas, demandez de l'aide à un des
enseignants !**

## Récupération d'un projet existant depuis une archive ZIP

Nous allons maintenant voir comment récupérer l'archive d'un projet
Flask existant déjà fonctionnel. Le projet que nous récupérerons met
en oeuvre les éléments qui seront introduits au cours des prochaines
séances. Ce projet est hébergé sur la plateforme d'hébergement de code
source [Github](https://github.com), mais nous récupérerons son code
source sous forme d'archive, **le gestionnaire de version GIT n'est
pas au programme de ce module**. Cependant nous vous conseillons
vivement de vous faire un compte sur Github, et de l'utiliser pour
héberger vos futurs projets de développement logiciels.

Le projet d'exemple est hébergé sur Github:
[https://github.com/badock/FlaskSar2019ExampleApp/tree/session0](https://github.com/badock/FlaskSar2019ExampleApp/tree/session0)

Github permet de récupérer une archive de ce projet:
[https://github.com/badock/FlaskSar2019ExampleApp/archive/session0.zip](https://github.com/badock/FlaskSar2019ExampleApp/archive/session0.zip)

Nous vous demandons de:
1. Télécharger l'archive ZIP via le lien précédent
2. Extraire cette archive 
![extraction de l'archive zip](/assets/img/session0/screen_install_0.png)
3. Sur la page d'accueil PyCharm, cliquer sur **ouvrir**
![extraction de l'archive zip](/assets/img/session0/screen_install_1.png)
4. Sélectionner le dossier où vous avez extrait l'archive ZIP et cliquez sur OK
![extraction de l'archive zip](/assets/img/session0/screen_install_2.png)
5. Attendre que PyCharm charge le projet:
![extraction de l'archive zip](/assets/img/session0/screen_install_3.png)
6. Lancer le projet. **Si vous rencontrez une erreur, prévenez un des enseignants!**

## Configuration du projet avec le mode _Flask_ et le mode _deboggage_

1. Cliquer sur **Ajouter une configuration**
![capture d'écran montrant comment configurer le mode flask et le mode debug](/assets/img/session0/flask_debug_0.png)
2. Sélectionner **Flask server**
![capture d'écran montrant comment configurer le mode flask et le mode debug](/assets/img/session0/flask_debug_1.png)
3. Sélectionner **Script path** et cliquer sur l'icône "Dossier"
4. Choisir le fichier **app.py** dans le dossier de votre projet Flask et cliquer sur **OK**
![capture d'écran montrant comment configurer le mode flask et le mode debug](/assets/img/session0/flask_debug_2.png)
5. Cocher **FLASK_DEBUG** et cliquer sur **OK**
6. Vérifier qu'un environnement d'exécution est bien sélectionné:
![capture d'écran montrant comment configurer le mode flask et le mode debug](/assets/img/session0/flask_debug_3.png)
7. Lancer le projet et regarder si une erreur apparait. **Si une erreur apparait, contactez un des enseignants**
![capture d'écran montrant comment installer les dépendances logicielles](/assets/img/session0/screen18.png)


