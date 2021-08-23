# TP Docker : Les commandes de base

**Exercice 1 : Hello from Alpine**
Le but de ce premier exercice est de lancer des containers basés sur l'image alpine
1. Lancez un container basé sur alpine en lui fournissant la commande echo hello
2. Quelles sont les étapes effectuées par le docker daemon ?
3. Lancez un container basé sur alpine sans lui spécifier de commande. Qu’observez-vous ?



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

**Exercice 3 : foreground / background**
Le but de cet exercice est de créer des containers en foreground et en background
1. Lancez un container basé sur alpine en lui spécifiant la commande ping 8.8.8.8
2. Arrêter le container avec CTRL-C
Le container est t-il toujours en cours d’exécution ?
3. Lancez un container en background, toujours en lui spécifiant la command ping 8.8.8.8
Le container est t-il toujours en cours d’exécution ?

**Exercice 4 : liste des containers**
Le but de cet exercice est de montrer les différentes options pour lister les containers du
système
1. Listez les containers en cours d’exécutionEst ce que tous les containers que vous avez créés sont listés ?
2. Utilisez l’option -a pour voir également les containers qui ont été stoppés
3. Utilisez l’option -q pour ne lister que les IDs des containers (en cours d’exécution ou
stoppés)

**Exercice 5 : exec dans un container**
Le but de cet exercice est de montrer comment lancer un processus dans un container
existant
1. Lancez un container en background, basé sur l’image alpine. Spécifiez la commande
ping 8.8.8.8
et le nom ping avec l’option --name
2. Observez les logs du container en utilisant l’ID retourné par la commande précédente ou
bien le nom du container
Quittez la commande de logs avec CTRL-C
3. Lancez un shell sh, en mode interactif, dans le container précédent
4. Listez les processus du container
Qu'observez vous par rapport aux identifiants des processus ?

**Exercice 6 : cleanup**
Le but de cet exercice est de stopper et de supprimer les containers existants
1. Listez tous les containers (actifs et inactifs)
2. Stoppez tous les containers encore actifs en fournissant la liste des IDs à la commande
stop
3. Vérifiez qu’il n’y a plus de containers actifs
4. Listez les containers arrêtés
5. Supprimez tous les containers
6. Vérifiez qu’il n’y a plus de containers
