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