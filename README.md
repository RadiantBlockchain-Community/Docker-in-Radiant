# Docker-in-Radiant
Docker scripts for Radiant blockchain

## Create image
```
docker build -t <name-image> .
```

## Run image

```
docker run -it -v <name-volume>:/data --name <name-container> <name-image>
```