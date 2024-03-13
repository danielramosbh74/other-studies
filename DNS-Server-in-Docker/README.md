# Source
[Running a DNS Server in Docker](https://medium.com/nagoya-foundation/running-a-dns-server-in-docker-61cc2003e899)

# Docker commands

docker network create --subnet=172.20.0.0/16 nagoya-net

docker build -t bind9 .

docker run -d --rm --name=dns-server --net=nagoya-net --ip=172.20.0.2 bind9

<!-- Enter inside the container and configure files:
named.conf.options
named.conf.local
db-nagoya-foundation.com
as they are here in VS Code -->
docker exec -it dns-server bin/bash

## After configure
docker exec -d dns-server /etc/init.d/bind9 start

docker run -d --rm --name=host1 --net=nagoya-net --ip=172.20.0.3 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"

docker run -d --rm --name=host2 --net=nagoya-net --ip=172.20.0.4 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"

<!-- Verify if there are 3 containers: 1 dns-server, host1 and host2 -->
docker ps -a
<!-- inside the host1 container: -->
docker exec -it host1 bash
<!-- After install ping: apt install -y iputils-ping -->
ping host2.nagoya-foundation.com

