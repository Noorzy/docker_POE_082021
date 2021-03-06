# TP Docker : Les commandes de base

**Exercice 1 : Hello from Alpine**
Le but de ce premier exercice est de lancer des containers basés sur l'image alpine
1. Lancez un container basé sur alpine en lui fournissant la commande echo hello
2. Quelles sont les étapes effectuées par le docker daemon ?
3. Lancez un container basé sur alpine sans lui spécifier de commande. Qu’observez-vous ?

----

**Correction de l'exercice 1**

1. La commande à lancer est :

```bash
$ sudo docker container run alpine echo hello
```

2. Le résultat de la commande précédente montre les différentes étapes qui sont effectuées:

```bash
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
88286f41530e: Pull complete
Digest: sha256:1072e499f3f655a032e88542330cf75b02e7bdf673278f701d7ba61629ee3ebe
Status: Downloaded newer image for alpine:latest
hello
```

L'image alpine n'étant pas disponible localement, elle est téléchargée depuis le Docker Hub.

Un container est alors lancé à partir de cette image avec la commande spécifiée (echo hello"). 
Le résultat de cette commande est affiché sur la sortie standard. Une fois la commande terminée, le container est arrêté

3. La commande à lancer est :
```bash
$ sudo docker container run alpine
```

L'image alpine étant déjà présente localement (téléchargée lors de l'étape précédente), elle est
ré-utilisée. La commande exécutée par défaut lors du lancement d'un container basé sur alpine est "/bin/sh". Cette commande ne produit pas de résultat sur la sortie standard. Le container est arrêté une fois la commande lancée.
Cette commande est prévu dans la conception de l'image.



**Exercice 2 : shell intéractif**
Le but de cet exercice est lancer des containers en mode intéractif
1. Lancez un container basé sur alpine en mode interactif sans lui spécifier de commande
2. Que s’est-il passé ?
3. Quelle est la commande par défaut d’un container basé sur alpine ?
4. Naviguez dans le système de fichiers
5. Utilisez le gestionnaire de package d’alpine (apk) pour ajouter un package

    ```
    $ apk update
    $ apk add curl
    ```

**Correction de l'exercice 2**

1. La commande permettant de lancer un container en mode "interactif" est la suivante:

    ```bash
    $ docker container run -ti alpine
    ```

2. Nous obtenons un shell sh dans le container après instanciation

3. Par défaut, la commande par défaut utilisée dans l'image alpine est */bin/sh*

    > Note: nous y reviendrons plus tard mais il est intéressant de voir que cette information est
présente dans le fichier Dockerfile qui est utilisé pour créer l'image
(https://github.com/alpinelinux/docker-alpine/blob/37579d92b9faa70398240431bc46720242faa5e5/x86_64/Dockerfile)

4. Nous pouvons naviguer dans le système de fichiers de la même façon que nous le faisonsdans une autre distribution Linux plus "traditionnelle" en utilisant les commandes cd, ls,
pwd, cat, less, ...

5. Le gestionnaire de package d'une distribution alpine est apk
Pour mettre à jour la liste des packages, nous pouvons utiliser la commande *apk update* .

Pour installer un package, comme curl , nous utilisons la commande suivante *apk add curl*


**Exercice 3 : foreground / background**
Le but de cet exercice est de créer des containers en foreground et en background
1. Lancez un container basé sur alpine en lui spécifiant la commande ping 8.8.8.8
2. Arrêter le container avec CTRL-C
Le container est t-il toujours en cours d’exécution ?
3. Lancez un container en background, toujours en lui spécifiant la command ping 8.8.8.8
Le container est t-il toujours en cours d’exécution ?

**Correction de l'exercice 3**

1. Le container peut être lancé avec la commande suivante :

```bash
$ docker container run alpine ping 8.8.8.8
```
2. La commande 'docker container ls' ne liste pas le container car le processus a été arrêté par la commande CTRL-C

3. La commande suivante permet de lancer le container en background :

```bash
$ docker container run -d alpine ping 8.8.8.8
```

> Le conteneur reste en exécution


**Exercice 4 : liste des containers**
Le but de cet exercice est de montrer les différentes options pour lister les containers du
système
1. Listez les containers en cours d’exécution. Est ce que tous les containers que vous avez créés sont listés ?
2. Utilisez l’option -a pour voir également les containers qui ont été stoppés
3. Utilisez l’option -q pour ne lister que les IDs des containers (en cours d’exécution ou
stoppés)

**Correction de l'exercice 4**:
1. Pour lister les containers en cours d'execution sur le système, les 2 commandes suivantes sont équivalentes
   
```bash
$ docker container ls
$ docker ps
```

> Note: la seconde commande est historique et date de l'époque ou la plateforme Docker était
principalement utilisée pour la gestion des containers et des images. Par la suite, d'autres
primitives que nous verrons dans les prochains chapitres ont été ajoutées (volume, network,
node, ...), ce qui a conduit à une refactorisation de l'api disponible en ligne de commande.

Seuls les containers en cours d'execution sont listés. Les containers qui ont été créés
puis arrêtés ne sont pas visible dans la sortie de cette commande.

2. Les commandes suivantes permettent de lister les containers en cours d'execution ainsi
que ceux qui sont stoppés.

```bash
$ docker container ls -a
$ docker ps -a
```

3. Les commandes suivantes permettent de lister les identifiants de l'ensemble des containers tournant sur la machine hôte, ceux en cours d'execution et également ceux qui
ont été stoppés.

```bash
$ docker container ls -aq
$ docker ps -aq
```


**Exercice 5 : exec dans un container**
Le but de cet exercice est de montrer comment lancer un processus dans un container
existant
1. Lancez un container en background, basé sur l’image alpine. Spécifiez la commande
ping 8.8.8.8 et le nom ping avec l’option --name
2. Observez les logs du container en utilisant l’ID retourné par la commande précédente ou
bien le nom du container
Quittez la commande de logs avec CTRL-C
3. Lancez un shell sh, en mode interactif, dans le container précédent
4. Listez les processus du container
Qu'observez vous par rapport aux identifiants des processus ?

**Correction de l'exercice 5**:

1. Lancez un container en background, basé sur l’image alpine. Spécifiez la commande
ping 8.8.8.8
et le nom ping avec l’option --name

```bash
$ sudo docker container run -d --name ping alpine ping 8.8.8.8
```

2. observer les logs 

```bash
$ sudo docker container logs -tf ping
```

3. Mode interactif :

```bash
$ sudo docker container exec -it ping /bin/sh
```

4. Lister les processus

```bash
# ps -ef
# top
```

**Exercice 6 : cleanup**
Le but de cet exercice est de stopper et de supprimer les containers existants
1. Listez tous les containers (actifs et inactifs)
2. Stoppez tous les containers encore actifs en fournissant la liste des IDs à la commande
stop
3. Vérifiez qu’il n’y a plus de containers actifs
4. Listez les containers arrêtés
5. Supprimez tous les containers
6. Vérifiez qu’il n’y a plus de containers

**Correction de l'exercice 6**
1. Afin de lister les containers qui tournent ainsi que ceux qui ont été arrêtés, il faut utiliser
l'option -a , les commandes suivantes sont équivalentes:

```bash
$ docker ps -a
$ docker container ls -a
```

2. Stopper tous les conteneurs actifs :

> On va utiliser la substitution de commande pour injecter la liste des conteneurs actif directement dans la commande de stop

```bash
$ sudo docker container stop $(sudo docker container ls -q)
```

3. Vérifiez qu’il n’y a plus de containers actifs

```bash
$ sudo docker container ls
$ sudo docker container ls -q
```

4. Listez les containers arrêtés

```bash
$ sudo docker container ls -a
$ sudo docker container ls -a
```

5. Supprimez tous les containers arrêtés

```bash
$ sudo docker container rm $(sudo docker container ls -aq)
```

> Mode prune :

```bash
$ sudo docker container prune
```


**Exercice 7 : publication de port**
Le but de cet exercice est de créer un container applicatif nginx en background et en exposant un port sur la machine hôte
1. Lancez un container basé sur nginx en background (detach) et publiez le port 80 du container sur le port 8080 de
l’hôte
2. Vérifiez depuis votre navigateur que la page par défaut de nginx est servie sur
http://ip_docker_host:8080
3. Lancez un second container en publiant le même port
Qu’observez-vous ?



**Correction de l'exercice 7**:

1. Lancez un container basé sur nginx en background (detach) et publiez le port 80 du container sur le port 8080 de
l’hôte

```bash
$ sudo docker container run -d -p 8080:80 nginx
```

2. Vérification

```http
http://172.28.128.86:8080
```

3. Lancez un second container en publiant le même port
Qu’observez-vous ?

> On ne peut pas publier deux fois le même port sur le docker hôte

```bash
docker: Error response from daemon: driver failed programming external connectivity on endpoint kind_grothendieck (797ba1f6ba5308ab32b198e7b98b7aed88e335abcc312bddc27436fe4d81d037): Bind for 0.0.0.0:8080 failed: port is already allocated.
```

**Exercice 8 : inspection d'un container Le but de cet exercice est l'inspection d’un container**
1. Lancez, en background, un nouveau container basé sur nginx:1.18 en publiant le port 80
du container sur le port 3000 de la machine host.
Notez l'identifiant du container retourné par la commande précédente.
2. Inspectez le container en utilisant son identifiant
    > Mot clé : inspect
3. En utilisant le format Go template, récupérez le nom et l’IP du container
4. Manipuler les Go template pour récupérer d'autres information


**Correction de l'exercice 8**:

1. Lancez, en background, un nouveau container basé sur nginx:1.18 en publiant le port 80
du container sur le port 3000 de la machine host.

```bash
$ sudo docker container run -d -p 3000:80 nginx:1.18
```

2. Inspection 

```bash
$ sudo docker container inspect aaa01d1df6d5fb41a669ddbff88afc4f24c75a36ca2c383d758c26dbb3a81ac2
```

3. Format GO template

```bash
$ sudo docker container inspect -f "{{ .Name }}"  aaa01d1df6d5fb41a
```

```bash
$ sudo docker container inspect -f "{{ .NetworkSettings.IPAddress }}" aaa01d1df6d5fb41a
```
