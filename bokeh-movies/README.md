# Application Bokeh : explorateur de films

## Présentation

L'objectif de cet exercice est de se familiariser avec la construction des images Docker et leur instanciation en conteneur. La technologie choisie est Bokeh, un framework web Python très utilisé pour réaliser des visualisations de Data Science. L'application choisie est un explorateur de films, tiré des exemples Bokeh et s'appuyant sur un échantillon de données IMDB. L'exercice suivra les étapes suivantes:
- Choix et récupération d'une image Docker de base Python 3.7 issue du Dockerhub
- Analyse de l'image afin de comprendre la base de travail
- Ajout des nouvelles couches d'images pour installer l'application
- Instanciation de l'image en conteneur
- Accès à l'application web via un navigateur web

## Description de l'application Bokeh

### Dépendances

Pour fonctionner, l'application a besoin des librairies Python *pandas* et bien sûr *bokeh*. L'application ne peut pas démarrer sans ses données, qui peuvent être téléchargées à partir de la commande:
`python -c "import bokeh.sampledata; bokeh.sampledata.download()"`

La version de Python choisie pour cet exercice est Python 3.7.

### Architecture
Le code de l'application est contenu dans le répertoire *src/*. Ce dossier contient le code Python, les requêtes SQL, le code HTML et des métadonnées.

Les données concernant les films doivent se situer dans le dossier */home/\<user\>/.bokeh*. \<user\> représente l'utilisateur qui lance l'application

### Utilisation

L'application est démarée à l'aide de la commande `bokeh serve --show <src_directory>`. Par défaut, elle est accessible à l'adresse: http://127.0.0.1:5006

## Exigences du packaging docker

Les exigences pour l'application Bokeh sont les suivantes
- L'application est accessible en HTTP sur le port 80 (port standard HTTP)
- Dans le conteneur, l'utilisateur applicatif n'est pas *root* (bonne pratique de sécurité)
- Les données sont indépendantes de l'image (respect de l'indépendance du service et de ses données)

## Choix et analyse de l'image de base

Pour la construction de l'image Docker, il faut choisir une image de base robuste et ne contenant que le strict nécessaire. En vérité, il existe une image Docker Bokeh officielle maintenue par l'équipe de Bokeh. Cette image de base serait toute indiquée. Mais pour l'intêret de l'exercice, nous ferons l'installation de bokeh par nous-même. Partir d'un image très bas niveau comme "Ubuntu" ou "Alpine" nécessiterait trop d'opérations et conduirait probablement à l'applicationd de mauvaises pratiques. Nous partirons donc d'une image Python 3.7. Procéder aux étapes suivantes:
- Se rendre sur le Dockerhub dans le dépôt Python: https://hub.docker.com/_/python
- Examiner les tags disponibles et choisir un tag approprié (le tag 3.7)
- Faire un *pull* de l'image choisie: `docker pull python:3.7`
- Examiner l'image téléchargée: `docker inspect python:3.7`. Trouver des informations utiles telles que l'OS, les variables d'environnement, la commande de lancement, etc.
- Explorer l'image en instanciant un conteneur avec un terminal interactif: `docker run -it python:3.7 bash`

## Construction de l'image

Maintenant que la base de travail est définie, nous allons ajouter des couches à l'image python 3.7 pour packager notre application. Pour cela:

- Editer le fichier *Dockerfile*

- Utiliser les mots clés Docker pour réaliser les instruction en commentaire (une commande par commentaire). Les mots clé à utiliser sont les suivants:
  + FROM <image:tag> : pour indiquer l'image de base
  + COPY \<fichier ou dossier source local\> \<destination dans l'image\> : pour ajouter des fichiers à l'image
  + USER <username>: pour changer l'utilisateur courant
  + RUN <commande shell> : exécuter une commande shell dans l'image (exemple: `RUN pip install ...`)
  + EXPOSE <port number>: déclarer un port qui sera utilisé
  + ENTRYPOINT <commande shell>: définir la commande à exécuter au démarrage du conteneur

- Construire l'image : `docker build --tag bokeh-movies:v0.0 .`

## Créer un conteneur à partir de l'image

Une fois l'image construite, il s'agit maintenant de définir la commande d'instanciation de l'image répondant à nos exigences. On utilise pour cela la commande `docker run [options] image:tag` pour un conteneur à partir de l'image et le démarrer. Dans notre cas, les options à utiliser sont:
 + "-p <port_exposé>:<port_interne>": pour exposer des ports de l'application
 + "-v <dossier_local>:<dossier_conteneur>": pour monter des volumes locaux dans le conteneur
 + "--name <nom>": pour régler le nom du conteneur

Lancer le conteneur et vérifier l'état de l'application dans les logs. Commande d'affichage des logs: `docker logs <nom_du_conteneur>`

## Test:

- Ouvrir le navigateur et se rendre à l'adresse: http://127.0.0.1:80 


