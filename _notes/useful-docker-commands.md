---
title: 'Useful Docker Commands'
topics:
  - docker
---

## List things

* Containers:

  `docker ps -a`

* Images:

  `docker images -a`

* Volumes:

  `docker volume ls `

* Networks:

  `docker network ls`

## Remove things

* Containers:

  `docker rm <container>`

* Images:

  `docker rmi <image>`

* Volumes:

  `docker volume rm <volume>`

* Networks:

  `docker network rm <network>`

## Start Container

`docker start <container>`

## Stop Containers

* Gracefully:

  `docker stop <container>`

* Forced:

  `docker kill <container>`

* All:

  `docker stop|kill $(docker ps -qa)`

## Update and Stop Crash Looping Container

`docker update --restart=no && docker stop`

## Copy files into container

`docker cp <path/on/host> [container]:<path/in/container>`

## Copy files from container

`docker cp [container]:<path/in/container> <path/on/host>`

## Connect to running container

* As current user

  `docker exec -i -t /bin/sh`

* As different user:

  `docker exec -i -t -u root /bin/sh`

## Cleanup Scripts

### Nuclear Option

```bash
docker kill $(docker ps -qa)
docker rm $(docker ps -qa)
docker rmi $(docker images -qa)
docker volume rm $(docker volume ls -q)
docker network rm $(docker network ls -q)
docker builder prune
```