# Docker Manual

## Table of Contents

* [Concept](#concept)
  * [Build](#build)
  * [Ship](#build)
  * [Run](#build)
* [Install docker engine on Ubuntu](#install-docker-engine-on-ubuntu)
* [Install docker compose on Ubuntu](#install-docker-compose-on-ubuntu)
* [Install docker machine on Ubuntu](#install-docker-machine-on-ubuntu)
* [Docker machine](#docker-machine)
* [Reference](#reference)
* [Tool](#tool)

## Concept

### Build

```bash
# Build an image from the Dockerfile in the current directory and tag the image
docker build -t myapp:1.0 .

# List all images that are locally stored with the Docker engine
docker images

# Delete an image from the local image store
docker rmi alpine:3.4
```

### Ship

```bash
# Pull an image from a registry
docker pull alpine:3.4

# Retag a local image with a new image name and tag
docker tag alpine:3.4 myrepo/myalpine:3.4

# Log in to a registry (the Docker Hub by default)
docker login my.registry.com:8000

# Push an image to a registry
docker push myrepo/myalpine:3.4
```

### Run

```bash
# Run container
docker run
  --rm  //remove container automatically after it exits
  --it //connect the container to terminal
  --name web //name the container
  -p 80:80 //expose port 80 externally and map to port 80
  -v /dev:/code //create a host mapped volume inside the container
  alpine:3.4 //the image from which the container is instantiated
  /bin/bash //the command to run inside the container

# Stop a running container through SIGTERM
docker stop web

# Stop a running container through SIGKILL
docker kill web

# Create an overlay network and specify a subnet
docker network create --subnet 10.1.0.0/24
--gateway 10.1.0.1 -d overlay mynet

# List the networks
docker network ls

# List the running containers
docker ps

# Delete all running and stopped containers
docker rm -f $(docker ps -aq)

# Create a new bash process inside the container and connect it to the terminal
docker exec -it web bash

# Print the last 100 lines of a container’s logs
docker logs --tail 100 web
```

## Install docker engine on Ubuntu

https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository

```bash
sudo apt-get remove docker docker-engine docker.io
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce

sudo usermod -aG docker ${USER}

su - ${USER}
```

## Install docker compose on Ubuntu

https://docs.docker.com/compose/install/#prerequisites

```bash
# version 18
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

# set mod
sudo chmod +x /usr/local/bin/docker-compose
```

## Install docker machine on Ubuntu

https://docs.docker.com/machine/install-machine/

```bash
# curl docker-machine
curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && \
chmod +x /tmp/docker-machine && \
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

## Docker machine

> คำสั่งที่ใช้งานบ่อยๆ

```bash
# Provision machine on virtualbox
# Reference https://docs.docker.com/machine/drivers/virtualbox/
docker-machine create -d virtualbox --virtualbox-disk-size=10000 manager

# show machine list
docker-machine ls

# remove machine
docker-machine rm your-machine-name

# dm = docker machaine
docker-machine -h
docker-machine ssh <dm name>
docker-machine inspect <dm name>
docker-machine ip <dm name>
docker-machine env <dm name>

# help
docker-machine COMMAND --help

# clear cache mahcine
sudo rm -rf ~/.docker/machine/machines/your-machine-name
```

## Docker node

```bash
# show node
docker node ls

# add label
docker node update --label-add <key>=<value> <node-id>
```

## Docker swarm

```bash
# docker swarm init
docker swarm init --advertise-addr your-ip-machine

# show token manager
docker swarm join-token manager

# help
docker swarm COMMAND --help
```

## Docker service

```bash
# create service
docker service create \
  --name=viz \
  --publish=8080:8080/tcp \
  --constraint=node.role==manager \
  --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
  dockersamples/visualizer

# remove service
docker service rm service-name
```

## Docker stack

```bash
# create stack
docker stack deploy -c docker-compose.yml stack-name

# remove stack
docker stack rm stack-name
```

## Reference

* [training.play-with-docker.com](http://training.play-with-docker.com/alacart/)

## Tool

* [Labs playground winth docker](http://labs.play-with-docker.com/)
