1. Build image

```bash
$ sudo docker image build -t iperf:1.0 .
```
2. Instanciation conteneur iperf serveur

```bash
$ sudo docker run --rm -it iperf:1.0
```

3. Instanciation conteneur iperf client 

```bash
$ sudo docker run --rm -it iperf:1.0 -c 172.17.0.2 -i 2 -t 20  -p 5201
```