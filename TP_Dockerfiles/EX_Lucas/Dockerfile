# Instruction FROM -> image parente
FROM debian:buster

# Instruction RUN
RUN apt-get update -yq \
&& apt-get install ca-certificates -yq \
&& apt-get install speedtest-cli -yq \
&& apt-get clean -y

# Instruction CMD -> commande exécutée par défaut par le conteneur instancié
CMD speedtest-cli