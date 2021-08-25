# Imaginer un Dockerfile

https://docs.docker.com/engine/reference/builder/

1. Creer un fichier Dockerfile

> Imaginer une image qui exécute un script bash que vous avez ajouté dans l'image (echo "Mon appli")

> Imaginer une image qui dispose de packages utiles pour vous

 - Instruction FROM

 - Instruction LABEL

 - Instruction RUN

 - Instruction COPY/ADD

 - Instruction CMD


2. Builder l'image :

https://docs.docker.com/engine/reference/commandline/build/

```bash
$ sudo docker image build -t monimage:1.0 .
```