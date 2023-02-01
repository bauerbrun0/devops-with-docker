## Part 1

### Definitions and basic concepts

#### Exercise 1.1: Getting started

```bash
$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS                     PORTS     NAMES
732738ecfc17   nginx     "/docker-entrypoint.…"   6 minutes ago   Exited (0) 5 minutes ago             suspicious_lamarr
fad8c55574bf   nginx     "/docker-entrypoint.…"   6 minutes ago   Exited (0) 5 minutes ago             eager_torvalds
8f9232f4f6a2   nginx     "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes               80/tcp    inspiring_almeida

```

#### Exercise 1.2: Cleanup

```bash
$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
$ sudo docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

### Running and stopping containers

#### Exercise 1.3: Secret message
Terminal 1
```bash
$ sudo docker run --name logger devopsdockeruh/simple-web-service:ubuntu
```
Terminal 2
```bash
$ sudo docker exec -it logger bash
$ tail -f ./text.log 
```
Secret message: `You can find the source code here: https://github.com/docker-hy`

#### 1.4
Terminal 1
``` bash
$ sudo docker run --name curler ubuntu sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
```
Terminal 2
``` bash
$ sudo docker exec -it curler bash
$ apt update
$ apt install curl
$ exit
```

### In-depth dive to images

#### Exercise 1.5: Sizes of images
Terminal 1
``` bash
$ sudo docker pull devopsdockeruh/simple-web-service:alpine
$ sudo docker images | grep simple-web-service
devopsdockeruh/simple-web-service   ubuntu    4e3362e907d5   22 months ago   83MB
devopsdockeruh/simple-web-service   alpine    fd312adc88e0   22 months ago   15.7MB
$ sudo docker run --name logger-alpine fd3
```
Terminal 2
``` bash
$ sudo docker exec -it logger-alpine sh
$ tail -f ./text.log
```

#### Exercise 1.6: Hello Docker Hub
``` bash
$ sudo docker run -it devopsdockeruh/pull_exercise
```
password: `basics`
(obtained: https://hub.docker.com/r/devopsdockeruh/pull_exercise)
secret message: `This is the secret message`

#### Exercise 1.7: Two line Dockerfile
Dockerfile
``` docker
FROM devopsdockeruh/simple-web-service:alpine
CMD server
```
``` bash
$ sudo docker build . -t web-server
$ sudo docker run web-server
```

#### Exercise 1.8: Image for script
Dockerfile
``` docker
FROM ubuntu:20.04

WORKDIR /usr/src/app

COPY curler.sh .

RUN apt update && apt install curl -y

CMD ./curler.sh
```
``` bash
$ sudo docker build . -t curler
$ sudo docker run -it curler
```