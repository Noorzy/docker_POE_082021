# Creation application micro-service docker

Le but de ce TP est d'assimiler les objets docker (image, container, network et volume) afin de déployer une application micro-service composée de technologies nginx, php et mariadb

## Creation du projet

1) Dans votre répertoire formationdocker :

    - Créer un nouveau répertoire nommé : TP_Appli_microservice
    - A partir du dépot de formation gitlab : https://github.com/psable/docker_POE_082021/tree/master/TP_Appli_microservice
        - recopier l'arborescence contenu dans le répertoire TP_Appli_microservice (répertoires et fichiers)

## Préparation network bridge

1) S'assurer que le réseau **mynetwork** soit créé
  - type : bridge


## Création d'un micro service de type php - nginx:

1) Instancier un conteneur myphp, voici les paramètres attendus :

  - name: myphp
  - detach
  - network : mynetwork
  - bind :
      -v /chemin/TP_Appli_microservice/php:/srv/http
  - image : phpdockerio/php73-fpm

2) Vérifier le contenu dans le conteneur php

3) Créer le conteneur mynginx avec les paramètres suivants :

  - name: mynginx
  - detach
  - network : mynetwork
  - bind :
      -v /chemin/TP_Appli_microservice/conf/nginx.conf:/etc/nginx/conf.d/default.conf
  - publication port : 8080:80
  - image : nginx:1.20-alpine

3) Test navigateur :

> http://ip_docker_hote:8080/