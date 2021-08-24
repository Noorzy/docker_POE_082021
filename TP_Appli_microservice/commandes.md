## Préparation network bridge

1) S'assurer que le réseau **mynetwork** soit créé

```bash
$ sudo docker network create mynetwork
```

## Création d'un micro service de type php - nginx:

1) Instancier un conteneur myphp, voici les paramètres attendus :

   ```bash
     # Attention : adapter le chemin absolu côté docker hote
     $ sudo docker container run -d --name myphp --network mynetwork -v /vagrant/TP_Appli_microservice/php/:/srv/http/ phpdockerio/php73-fpm
   ```

2) Vérifier le contenu dans le conteneur php
  
  ```bash
  $ docker container ls
  $ docker container logs myphp
  $ docker container exec myphp cat /srv/http/index.php
  ```

3) Créer le conteneur mynginx avec les paramètres suivants :

  ```bash
  $ docker container run -d --name mynginx -p 8080:80 -v /vagrant/TP_Appli_microservice/conf/nginx.conf:/etc/nginx/conf.d/default.conf --network mynetwork nginx:1.20-alpine
  $ docker container ls
  $ sudo docker container exec mynginx cat /etc/nginx/conf.d/default.conf
  $ sudo docker container logs mynginx
  ```

## Update php

```bash
$ sudo docker container stop myphp
$ sudo docker container rm myphp
$ sudo docker container run --name myphp -d --network mynetwork -v /vagrant/TP_Appli_microservice/php:/srv/http phpdockerio/php74-fpm
```

## Ajout mariadb

```bash
$ sudo docker image pull mariadb:10.5
```

- Pour ce genre d'application, il faut spécifier au moins une variable d'environnement à l'instanciation du conteneur 

```bash
$ sudo docker container run -d --name mybdd --network mynetwork -e MARIADB_ROOT_PASSWORD=roottor mariadb:10.5
$ sudo docker container ls
$ sudo docker container logs mybdd
```

- Test bdd :

```bash
$ sudo docker container exec -it mybdd bash
root@d8ac4f4ccb4a:/# 
root@d8ac4f4ccb4a:/# 
root@d8ac4f4ccb4a:/# mysql -u root -proottoor
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 10.5.12-MariaDB-1:10.5.12+maria~focal mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.001 sec)

MariaDB [(none)]>
```