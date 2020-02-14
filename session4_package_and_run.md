---
title: Session 4 - Package and run a new service with Docker
excerpt: ""
order: 4
---

1. TOC
{:toc}


In this Session we will see how the example application developed in the [Session 3](/session3_develop.html) can be packaged in a docker image and run by any host that has Docker installed.

# Developping a Dockerfile

To package the program we developed in [Session 3](session3_develop.html), let's create a file called `Dockerfile`
that has the following content:

```
FROM openjdk:15-jdk-slim

COPY bin bin

ENTRYPOINT java -classpath "bin/" -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:8000 Helloworld
```

In this Dockerfile we do the following steps:
- First we indicate which base image may be used via the `FROM` instruction.
- Second we copy the local bin folder of the virtual machine on the file system on the container : the container now has the last compiled version of the application.
- Finally, we define the command line that should at the startup of the container.


# Creating a docker image

To create the image, run the following command

```bash
docker build . -t lowcomote
```

The process of creating the image should start as follow:
<script id="asciicast-301205" src="https://asciinema.org/a/301205.js"  data-size="small" data-cols="100" data-rows="30" async></script>


the image should now be available on the list of images:

```bash
root@lowcomote-VirtualBox:/home/lowcomote/workspace# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
lowcomote           latest              d4178dde65ba        2 minutes ago       413MB
openjdk             15-jdk-slim         7850bfdb733e        2 days ago          413MB
```

# Running a container based on the docker image

Now that the `lowcomote` is created, we can now a container based on the `lowcomote` image. To do so, run the following command:

```bash
docker run --detach -p 8000:8000 --name lowcomote lowcomote
```

And check that the container is running:
```bash
root@lowcomote-VirtualBox:/home/lowcomote/workspace# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
2507834ab420        lowcomote           "/bin/sh -c 'java -c…"   8 seconds ago       Up 4 seconds        0.0.0.0:8000->8000/tcp   lowcomote
```

The output above confirms that the container has been created!

# *(Bonus) Publish the docker image*

Run a local registry on the virtual machine:

```bash
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

To upload the `lowcomote` image to the private repository, we will now tag it with its hostname and port:
```
docker tag lowcomote localhost:5000/lowcomote
```

Once the tagging is done, we can push the image to the private repository:
```bash
docker push localhost:5000/lowcomote
```

You can try to pull the image from the local repository as follow:
```bash
docker pull localhost:5000/lowcomote
```

Now, aks your neighbour for his IP address. Let's call it `MY_NEIGHBOUR_IP`, let's grab its version of the `lowcomote` image:
```bash
MY_NEIGHBOUR_IP=192.168.1.25
docker pull $MY_NEIGHBOUR_IP:5000/lowcomote
```

The image should have been downloaded correctly:
```bash
root@lowcomote-VirtualBox:/home/lowcomote# docker image ls
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
192.168.1.25:5000/lowcomote   latest              d4178dde65ba        29 hours ago        413MB
```

You can run your neighbour's image as follow:
```bash
MY_NEIGHBOUR_IP=192.168.1.25
docker run --detach -p 8000:8000 --name lowcomote $MY_NEIGHBOUR_IP:5000/lowcomote
```
