# Creation application micro-service docker

Le but de ce TP est d'assimiler les objets docker (image, container, network et volume) afin de déployer une application micro-service composée de technologies nginx, php et mariadb

## Creation du projet

1) Dans votre répertoire formation docker :

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
      -v /vagrant/TP_Appli_microservice/php:/srv/http
  - image : phpdockerio/php73-fpm

2) Vérifier le contenu dans le conteneur php

3) Créer le conteneur mynginx avec les paramètres suivants :

  - name: mynginx
  - detach
  - network : mynetwork
  - bind :
      -v /vagrant/TP_Appli_microservice/conf/nginx.conf:/etc/nginx/conf.d/default.conf
  - publication port : 8080:80
  - image : nginx:1.20-alpine

3) Test navigateur :

> http://ip_docker_hote:8080/

## Mise à jour d'un micro-service (mise à jour version)

- On demande de passer en version php74-fpm

juste recréer une nouvelle instance avec l'update qui utilise les mêmes volumes, comme ça on ne perd pas ce qu'on a modifié

pour update php sans perdre les modifs:

```bash
$ sudo docker container stop myphp
$ sudo docker container rm myphp
$ sudo docker container run --name myphp -d --network mynetwork -v /vagrant/TP_Appli_microservice/php:/srv/http phpdockerio/php74-fpm
```

## Ajout d'un micro service de type mariadb :

 - On souhaite integrer une base de données au projet

 > Analyser l'image (dockerhub) mariadb : https://hub.docker.com/_/mariadb

Découverte d'une image **mariadb**
1. Télécharger l'image mariadb:10.5
2. Instancier un conteneur à partir de cette image :
  - name: mybdd
  - detach
  - image : mariadb:10.5
  - network : mynetwork
  - /!\ variable d'environnement : MARIADB_ROOT_PASSWORD


## INtegration du nouveau micro-service mybdd dans le code php

- On modifie le code php et on recharge la page

- /!\ On obtient une erreur PHP : il manque un driver dans l'image phpdockerio/php73-fpm ou phpdockerio/php74-fpm


## On va debugger manuellement le pb

- Dans le conteneur myphp, installer manuellement le driver manquent

1. On se connecte dans le conteneur myphp et on install le package manquant :

  ```bash
  $ sudo docker container exec -it myphp bash
  $ apt update && apt install php7.4-mysql
   ```

2. On redémarre le conteneur myphp pour redémarrer l'appli php

> /!\ Ne jamais vouloir redémarrer une appli directement dans le conteneur

```bash
$ sudo docker container restart myphp
```

3. Les databases sont correctement vue à présent

4. On fusionne nos modification réalisée dans la layer R/W du conteneur (et qui peut être détruite) dans une nouvelle image qui pourra alors être utiliser en cas de reconstruction

```bash
$ sudo docker container commit -a "Pierre" -m "Ajout package mysql php" myphp myphp_mysql:7.4
$ sudo docker image ls
```

> Si on doit réinstancier le conteneur myphp, on utilisera la nouvelle image :

```bash
$ sudo docker container run --name myphp -d --network mynetwork -v /vagrant/TP_Appli_microservice/php:/srv/http myphp_mysql:7.4
```

## Analyse volumes et conteneur mybdd