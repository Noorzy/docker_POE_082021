# TP init docker-compose

https://docs.docker.com/compose/install/

https://docs.docker.com/compose/compose-file/

1. Créer un répertoire projet

    - Créer dans /vagrant u répertoire nommé **TP_Dockercompose**

2. Créer le fichier docker-compose.yaml qui instanciera un conteneur apache

    - Dans le répertoire **TP_Dockercompose**, créer un répertoire **projet-apache**
    - Dans ce répertoire, créer un fichier docker-compose.yaml avec la description suivante :
        - version: 2.4
        - Block **services** avec un conteneur httpd
            - image : httpd
            - port publié : 8005
            - hostname : monweb01

3. Déclencher le docker-compose

    - Se positionner dans le répertoire qui contient le fichier docker-compose.yaml
    
    - Commande docker-compose (sudo)

    ```bash
    $ sudo docker-compose up
    ```