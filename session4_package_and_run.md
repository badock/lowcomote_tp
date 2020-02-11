---
title: Session 4 - Package and run a new service with Docker
excerpt: ""
order: 4
---

1. TOC
{:toc}


In this Session we will see how the example application developed in the [Session 3](/session3_develop.html) can be packaged in a docker image and run by any host that has Docker installed.

# Developping a Dockerfile

```
FROM openjdk:15-jdk-slim

COPY bin bin

ENTRYPOINT java -classpath "bin/" -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:8000 Helloworld
```

# Creating a docker image

```bash
docker build . -t lowcomote
```

# Running a container based on the docker image

```bash
docker run --detach -p 8000:8000 --name lowcomote lowcomote
```

```bash
docker ps
```

# *(Bonus) Publish the docker image*

Run a local registry

```bash
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```


Pull an image from a third party repository:
```bash
docker pull localhost:5000/my-ubuntu
```
