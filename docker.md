docker ps -a

### Create image
```
docker build -t ceres-query-runner .
```

### local debug
```
docker run -it -d -v /Users/chris/.ssh/docker:/root/.ssh 215efa4d7356
```
### remote
*docker run -it -d -v /home/gitlab-runner/.ssh/gb-deploy:/root/.ssh 215efa4d7356*

docker exec -it e18f9d33e04b bash 

### Host

docker host url is: host.docker.internal

### deploy
```
docker tag 215efa4d7356 docker.generalbioinformatics.com/it/build/deploy
docker push docker.generalbioinformatics.com/it/build/deploy
```

## Airflow

### Remove all <none> images

```
docker image prune --filter="dangling=true"
```

### Sort docker ps -a
```
docker ps -a | tac
```

### Remove all stopped containers
```
docker rm $(docker ps -a -q)
```

### docker login

docker login docker.generalbioinformatics.com --username=cbishop

### check ports
lsof -i tcp:3000

