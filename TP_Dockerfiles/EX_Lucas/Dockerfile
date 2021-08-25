# Instruction FROM -> image parente
FROM debian:buster

# Instruction RUN
RUN apt-get update -yq \
&& apt-get install ca-certificates -yq \
&& apt-get install speedtest-cli vim curl -yq \
&& apt-get clean -y

RUN mkdir /monappli

ENV SPEEDBUFFERED=1

# Instruction CMD -> commande exécutée par défaut par le conteneur instancié
# Commande par defaut à l'instanciation d'un conteneur -> elle peut être surchargée/remplacée par une commande lors de l'instanciation du conteneur
CMD speedtest-cli