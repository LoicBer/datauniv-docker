# Exploration de la CLI Docker

## Présentation

La CLI Docker est un client en lignes de commandes qui permet d'interagir avec le démon Docker. L'utilisation de la CLI se fait à travers les commandes `docker ...`. Cet outil permet notamment de:
- Télécharger des images Docker
- Instancier des conteneurs à partir d'une image
- Gérer tout le cycle de vie des conteneurs (création, supervision, administration, destruction)
- Construire des images Docker
- Pousser des images vers une _registry_

Remarque importante: la commande `docker` donne indirectement un **accès complet au système hôte**. Par défaut, il faut les droits administrateurs pour l'utiliser. Alternativement, il est possible d'ajouter l'utilisateur au groupe _docker_.

## Exploration de l'image Python

Dans l'optique du TP suivant (Bokeh Movies), nous allons explorer l'image Docker Python officielle. Cette image nous servira de base à la construction de notre propre image Docker. Il est donc important de comprendre comment cette image est constituée, afin de pouvoir y rajouter nos propres couches.
Nous allons en particulier rechercher les informations suivantes:
- Quelle est la distribution de base (Ubuntu ? Centos ? Alpine ? Autre ?)
- Quelle commande est lancée lorsque le conteneur est démarré ?
- Quel est le répertoire de travail ?
- Quel est l'utilisateur par défaut (root ? un utilisateur non privilégié ?)
- Comment est installé python (chemin ? pip disponible ? modules déjà installés ?)
- Quelles sont les variables d'environnement


La majeure partie de ce travail peut être réalisée avec la CLI. Voici un cheminement possible:

### Identification via la CLI

1. Récupérer l'image python, version 3.7 depuis le Dockerhub
`docker pull python:3.7`
2. Afficher les images présentes localement
`docker images`
3. Instancier un conteneur, nommé py37 et interactif à partir de l'image
`docker run --name py37 -it python:3.7`
4. Afficher les conteneurs présents localement
`docker ps -a`
5. Supprimer le conteneur py37
`docker rm py37`
6. Inspecter l'image
`docker inspect python:3.7`
Repérer la commande lancée au démarrage et les variables d'environnement
7. Réinstancier un conteneur en surchargeant la commande de lancement : lancer un terminal bash
`docker run --rm --name py37 -it python:3.7 bash`
Repérer l'utilisateur par défaut et le répertoire de travail. Trouver la distribution utilisée : `cat /proc/version`

Nous avons maintenant toutes les informations nécessaires pour construire notre future application Python sur cette image.

### Identification via le Dockerhub et Github

Les mêmes informations peuvent généralement être trouvées via le Dockerhub et Github :
- https://hub.docker.com/_/python
- https://github.com/docker-library/python/blob/6cb0acf85a5d8f315e5e5f61a3909b1f7d7f4fca/3.7/buster/Dockerfile

 
