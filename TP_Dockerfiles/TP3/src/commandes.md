## Build image
$ sudo docker image build -t pythonsrv:1.0 .

## Instanciation conteneur
$ sudo docker container run -p 8002:8000 pythonsrv:1.1