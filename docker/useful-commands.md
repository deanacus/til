# Useful Docker CLI Commands:

Top 16 docker commands

## List Containers

List only running containers:

`docker ps `

List All Containers:

`docker ps -a`


## Pull Image From DockerHub

`docker pull `

download a image from Docker Hub registry. Link to the docker image is always shown on the right at dockerhub.

## Build Image


`docker build  .`

`docker build -t "image-tag" .`


## List Images

`docker image ls`


## Start A Container From An Image
docker run ${image}

## Print log output of a container

Print lastest logs
docker logs ${container}

Tail the logs
docker logs -f ${container}

docker cleanup

#stop everything:
  docker kill $(docker ps -q)
#
  docker rm $(docker ps -q)
  docker rmi $(docker images -q)
  docker volume rm $(docker volume ls -q)
  docker network rm $(docker network ls -q)
  docker builder prune


## List All Volumes
docker volume ls 

## Clean up Volumes
docker volume prune

## List Networks
docker network ls

## Add A Container To A Network
docker network connect ${container}
adds the container to the given container network. That enables container communication by simple container name instead of IP.

## Remove A Stopped Container
docker rm  ${container}
 removes one or more containers. docker rm mycontainer, but make sure the container is not running

## Remove An Image With No Running Containers
docker rmi ${image}
removes one or more images. docker rmi myimage, but make sure no running container is based on that image

## Stop A Running Container

Stop a single container:

docker stop  ${container}

Stop all running containers:

docker stop $(docker ps -a -q)


## Start A Stopped Container
docker start
- starts a stopped container using the last state


docker update --restart=no
updates container policies, that is especially helpful when your container is stuck in a crash loop

## Copy Files Into Or Out Of A Container
docker cp
to copy files from a running container to the host or the way around. docker cp :/etc/file . to copy /etc/file to your current directory.


## Kill all running containers:

```bash
docker kill $(docker ps -q)
```

## Delete all stopped containers with

```bash
docker rm $(docker ps -a -q)
```

## Delete All Images

```bash
docker rmi $(docker images -q)
```

## Update and Stop Crash Looping Container

```bash
docker update --restart=no && docker stop
```

## Connect to shell in container

```bash
docker exec -i -t /bin/sh
```

If the container is running a different user account:

```bash
docker exec -i -t -u root /bin/sh
```
