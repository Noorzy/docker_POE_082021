# Instruction FROM : image parente qui me fournie le binaire directement (image officielle plutôt que de tout reconstruire)
FROM python:3.8-slim-buster

# Instruction LABEL -> metadata qui seront visible dans l'objet image buildée (inspect, partage de l'image dans le hub)
LABEL "formation.docker"="Pierre"
LABEL version="1.0"
LABEL description="Image demo dockerfile"

# Instruction ENV -> Definir une variable d'environnement
ENV SRC_DIR /usr/bin/src/webapp/src

# Instruction COPY -> ajouter des sources locales (projet Dockerfile) dans l'images
COPY src/server.py ${SRC_DIR}/server.py

# Instruction WORKDIR -> déplacer le répertoire courant
WORKDIR ${SRC_DIR}

ENV PYTHONUNBUFFERED=1

# Instruction CMD -> commande finale à exécuter par le conteneur qui ouvrira cette image
CMD ["python", "server.py"]