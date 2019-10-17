# Application mutli-conteneurs: jupyter et serveur git

## Présentation

L'objectif de cet exercice est d'instancier une application composée de plusieurs conteneurs afin de réaliser une mini architecture microservices. On utilisera pour cela l'outil **docker-compose** permettant de gérer plusieurs conteneurs simultanément. Les deux services sont à monter sont:
  - Jupyter: le célèbre serveur de notebook incontournable en data science
  - Gogs: un serveur de gestion de dépôts Git extrêmement léger

L'idée est de coder dans Jupyterhub et de gérer le *versioning* dans Gogs.

## Principe et architecture

Docker-compose remplace la commande `docker run`. Les options habituellement passées à `docker run` sont maintenant décrites à l'identique dans le fichier de configuration `docker-compose.yaml`. La commande `docker-compose up` permet d'instancier les conteneurs décrits dans ce fichier de configuration. Docker-compose se chargera de créer un réseau local privé pour que les conteneurs puisse dialoguer entre eux en utilisant leur nom de service.

- Jupyter écoute en HTTP sur le port 8888. Il est accessible par Gogs à l'adresse http://jupyter:8888/
- Gogs écoute en HTTP sur le port 3000. Il est accessible par Jupyter à l'adresse http://gitgogs:3000/

## Réalisation

### Étapes préléminaires

1. Mettre à jour de dépôt local git
  - `cd ~/datauniv-docker`
  - `git fetch`
  - `git pull origin master`

2. Se placer dans le répertoire de l'exercice
  - `cd jupyter-docker-compose`

3. S'assurer que docker-compose est installé
  - `sudo apt install docker-compose`

4. Décompresser jupyter-data
  - `tar -xvzf jupyter-data.tgz`

### Instanciation des conteneurs

5. Consulter le fichier `docker-compose.yaml`
  - `nano docker-compose.yaml`
  - Repérer et comprendre les 2 services à créer

6. Utiliser docker-compose pour lancer les applis
  - `docker-compose up`
  - observer les logs de lancement des applications

### Accès aux applications

Les étapes suivantes sont à faire dans un navigateur internet

7. Configurer Gogs
  - Se connecter à http://127.0.0.1:3000
  - Choisir la base de donnée "SQLite3"
  - Définir l'url de l'application à "http://gitgogs:3000"
  - Créer l'utilisateur admin:
    - name: datauniv
    - password: datauniv
    - mail: datauniv@data.univ
  - Installer gogs
  - Se reconnecter à http://127.0.0.1:3000
  - Créer un dépot "data-project"

8. Accéder à Jupyter
  - Dans les logs docker-compose, noter le token Jupyter
  - Se connecter à http://127.0.0.1:8888
  - Entrer le token
  - Naviguer dans persist/data-project
  - ouvrir un terminal Jupyter et dans ce terminal, exécuter les commandes:
    - `cd persist/data-project`
    - `git push origin master`
    - Entrer les identifiants Gogs: datauniv/datauniv

9. Dans l'interface de Gogs, vérifier que le code a bien été poussé

