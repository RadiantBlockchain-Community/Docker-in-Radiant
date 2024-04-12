# Docker-in-Radiant
**Dockerhub:** https://hub.docker.com/u/radiantcommunity

Docker scripts for Radiant blockchain.d

## Create image
```
docker build -t <name-image> .
```

## Run image

```
docker run -it -v <name-volume>:/data --name <name-container> <name-image>
```

### List volumes
Allows to know which volumes are created in the system.

```
docker volume ls
```

### List containers
Indicates the containers created in the system

```
docker container ps
```

### Know the ip of the container to use with Electrum

```
docker inspect <id_container or name_container>
```

### Entering a container
During the execution of a container, it is possible to enter it from another terminal.

```
docker exec -it <container_id_or name_container> /bin/bash
```

### Reopen a container
When a container is closed and you want to continue where it was.

```
docker start <container_id>
```

### Stop containers

```
docker kill <id_container>
```

### Delete containers and images

```
docker system purge --all
```