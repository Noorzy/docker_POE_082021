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

> https://docs.docker.com/storage/volumes/


## Networking 

> https://docs.docker.com/network/

- Par défaut, tous les conteneurs instanciés ont le réseau de type bridge docker0 ET sont donc tous dans le même VLAN
    - Ils peuvent acceder à l'exterieur, on peut les joindre (via une publication de port) ET ils peuvent communiquer entre eux par IP (pas de DNS)

- 


