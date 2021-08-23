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


