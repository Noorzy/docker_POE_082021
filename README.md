# Formation Docker

> Docker initiation

## Liens docs

https://docs.docker.com/

> Install Docker Engine (server) sur ubuntu

https://docs.docker.com/engine/install/ubuntu/

## Installation du docker engine 

- Via le script fourni (mode maquette mais peut servir d'exemple)

  ```bash
  $ curl -fsSL https://get.docker.com -o get-docker.sh
  $ sudo sh get-docker.sh
  ```

- Test client docker :

    - /!\ Un utilisateur standard n'a pas assez de privilèges pour interroger le daemon docker
    - Il est conseillé de passer par SUDO (élévation de privilèget et AUDIT)

    ```bash
    $ sudo docker version
    ```

- Vérification du service docker (daemon docker)

    ```bash
    $ systemctl status docker
    ```

- Vérification config du daemon docker 

    ```bash
    $ sudo docker system info
    ```

## Aide en ligne de commande 

- Aide sur l'utilisation d'un objet :

   ```bash
   $ sudo docker image --help
   $ sudo docker container --help
   ```

- Aide plus approfondie sur la commande exécutée pour un objet

   ```bash
   $ sudo docker image ls --help
   ```


## Images docker 

- Docker hub : registry publique

    > https://hub.docker.com/

- Téléchargement d'une image officielle "centos" en local sur le docker host

    ```bash
    $ sudo docker image pull centos
    ```

- Lister les images locales

    ```bash
    $ sudo docker image ls
    ```

## Containers Docker

- Instanciation d'un conteneur :

```bash
$ sudo docker container run centos:latest
```

> Par défaut un conteneur s'instancie, exécute la commande prévue par l'image et s'arrête si plus de processus à l'intérieur

- Instanciation d'un conteneur en modifiant la commande de base prévue par l'image 

```bash
$ sudo docker container run centos:latest echo "Formation docker"
Formation docker
```

- Mode interactif : prise de contrôle dans le conteneur

```bash
$ sudo docker container run -it centos:latest
```


- Mode détaché (background)

```bash
$ sudo docker container run -it -d centos:latest ping 8.8.8.8
```

- Prise de contrôle dans un conteneur déjà démarré

```bash
$ sudo docker container exec -it {container_name} /bin/bash
```

## Publication de ports

> Permet de mettre à disposition un conteneur applicatif de type web pour les utilisateur extérieur au docker hôte

> Voir TP1 ex7

https://docs.docker.com/config/containers/container-networking/


## Ajout de données dans un conteneur : CIBLE -> VOLUME/BIND

1. On peut se connecter dans le conteneur et créer/modifier  des données
    - /!\ : On écrit dans la layer R/W éphémere donc perdue si destruction du conteneur
    ```bash
    $ sudo docker container exec -it amazing_dewdney bash
    root@aaa01d1df6d5:/# cd /usr/share/nginx/html/
    root@aaa01d1df6d5:/usr/share/nginx/html# ls -l
    total 8
    -rw-r--r-- 1 root root 494 Apr 21  2020 50x.html
    -rw-r--r-- 1 root root 612 Apr 21  2020 index.html
    root@aaa01d1df6d5:/usr/share/nginx/html# echo "Coucou from docker" > index.html 
    root@aaa01d1df6d5:/usr/share/nginx/html# exit
    ```

2. On peut copier des fichiers dispo sur le docker hote dans un conteneur

    ```bash
    $ sudo docker container cp nginx/index.html amazing_dewdney:/usr/share/nginx/html/index.html
    ```

    - /!\ : On écrit dans la layer R/W éphémere donc perdue si destruction du conteneur
    - On peut retrouver l'état initial, en recréant un conteneur puis en recopier le fichier

3. Se servir des fonctionnalité docker type VOLUME/BIND

> https://docs.docker.com/storage/bind-mounts/

- EX: 
    ```bash
    $ sudo docker container run -d -p 3001:80 -v /vagrant/myweb:/usr/share/nginx/html nginx
    ```

> https://docs.docker.com/storage/volumes/

- EX:
    ```bash
    $ sudo docker container run -d -P -v mynginxvol:/usr/share/nginx/html nginx
    ```


## Networking 

> https://docs.docker.com/network/

- Par défaut, tous les conteneurs instanciés ont le réseau de type bridge docker0 ET sont donc tous dans le même VLAN
    - Ils peuvent acceder à l'exterieur, on peut les joindre (via une publication de port) ET ils peuvent communiquer entre eux par IP (pas de DNS)

- Un network bridge "user-define" est un network créé manuellement
    - Nouveau VLAN dédié
    - Mecanique DNS mise en place automatiquement avec les noms des conteneurs

- Network type "host" : le conteneur prend la config réseau du docker host

- Network type "none" : le conteneur n'a pas de stack réseau

- EX : création d'un network bridge et instanciation d'un conteneur dans ce network
    ```bash
    $ sudo docker network create mynet
    $ sudo docker container run  --network mynet --name alpine3 -it alpine
    ```


## Images Docker -> Dockerfile

> Concept : decrire un manifest qui vous permettra de construire une image à vos besoins

https://docs.docker.com/engine/reference/builder/

https://www.docker.com/blog/intro-guide-to-dockerfile-best-practices/

> Voir les exemple TP_Dockerfiles et les commandes

## Partage images 

- Solution : tar (save load)
    ```bash
    $ sudo docker image save -o iperf.tar iperf:1.0
    ```

- Solution Registry (depot d'images)

    - Publique : Docker hub
        - Il faut un compte (gratuit ou payant)
        - Etapes :
            - login : ```$ docker login```
            - tag : ```$ sudo docker image tag monalpine:1.0 bilbloke/monalpine:1.0```
            - push : ```$ sudo docker image push bilbloke/monalpine:1.0```

    - Privée : Cloud - SI
        - Registry privée docker : https://docs.docker.com/registry/

        - => Container registry : cloud (azure, google, ibm), gitlab, harbor


## Docker compose

> Description des micro-service à déployer sous forme de fichier yaml

- Intelligence pour la création des objets
- Beaucoup de possibilités orchestration, les dépendances...

https://docs.docker.com/compose/

https://docs.docker.com/compose/install/

1. Téléchargement du binaire :

```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. Positionnement des droits :

```bash
$ sudo chmod +x /usr/local/bin/docker-compose
```

3. Vérification

```bash
$ sudo docker-compose version
```