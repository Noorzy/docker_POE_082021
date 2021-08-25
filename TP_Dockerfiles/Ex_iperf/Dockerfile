FROM ubuntu:20.04

LABEL version=1.0
LABEL description="network benchmark(iperf)"

RUN apt-get update && apt-get dist-upgrade -yq \
	&& apt-get install -yq iperf3 netcat-openbsd \
	&& apt-get clean && rm -rf /var/lib/apt/lists/*

EXPOSE 5201

# Instruction HEALTHCHECK -> sonde applicative pour tester la dispo du port 5201
HEALTHCHECK --interval=1m --timeout=3s --retries=3 CMD nc -zv localhost 5201 || exit 1

# Instruction ENTRYPOINT
ENTRYPOINT ["/usr/bin/iperf3"]

# Instruction CMD après ENTRYPOINT -> CMD devient un argument de ENTRYPOINT
CMD ["-s"]

# Commande finale exécutée dans le conteneur :
# /usr/bin/iperf3 -s

# SI instanciation conteneur avec surcharge commande :
# $ sudo docker run --rm   iperf:1.0 whoami
# On tombe en erreur car la commande devient :
#/usr/bin/iperf3 whoami