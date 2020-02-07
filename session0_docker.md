---
title: Appendix - Demonstration on Docker containers
excerpt: ""
order: 6
---

# Demonstration on Docker containers
1. TOC
{:toc}

## Resources

Please install Docker on your laptop! See [this](https://docs.docker.com/docker-for-windows/install/) for Windows and MacOS. For Linux users you should easily find a documentation online according to your distribution!

## Check your installation

1. Open a terminal
2. try `docker --help`

## Pull an Ubuntu basic image from Docker Hub

1. take a look at the [image on Docker Hub](https://hub.docker.com/_/ubuntu/)

2. `docker pull ubuntu`

You've just pull an image from Docker Hub!

3. `docker image ls`

You can see the Ubuntu image on your list of images.

## Create a Dockerfile

A Dockerfile is a file containing a set of instruction to build a new Docker image. Common Dockerfile instructions start with `RUN`, `ENV`, `FROM`, `MAINTAINER`, `ADD`, and `CMD`, among others. They are equivalent to system commands.

1. create a new Dockerfile `touch your-path/Dockerfile`

2. open it with your favorite text editor (emacs, vi, sublimetext etc.)

3. copy/paste the following content in the Dockerfile

```
FROM ubuntu:latest

RUN apt-get -y update
RUN apt-get install sl -y
```

- The instruction `FROM ubuntu:latest` means that the new Docker image will be built by using ubuntu:latest as a basic layer;
- the other instructions run commands on the basic image before the new image is created, in this example the packages `sl` (remember the train?) is installed.

## Build a new Docker image and run it

1. `docker build -f Dockerfile -t demo-ubuntu .`

This command builds the new image, called `demo-ubuntu`, described in the Dockerfile `Dockerfile`.

2. `docker run -i -t demo-ubuntu /usr/games/sl`

Have you seen the train? Yes ! It means your new image have been successfully created and that you have been able to run a command on a container that runs this image.

## Get a bash on your container

1. `docker run -i -t demo-ubuntu /bin/bash`

You are know using a bash terminal on your container.

2. `apt-get install python`

Python is not installed in the basic Ubuntu image. Let's install it directly from the bash terminal on your container.

3. `python --help`

Ok Python have been installed successfully on the container!

4. `exit`

## WTF?

1. `docker run -i -t demo-ubuntu python --help`

What? You've just installed Python in your container but it is not working anymore? What is happening?

Do you remember the philosophy of Docker about application packaging? Do you remember the difference with LXC?

## Another image

Let's create another image to check what you observed.

1. Create another Dockerfile `toto` with the following content:

```
FROM ubuntu:latest

RUN apt-get -y update
RUN apt-get install sl -y
RUN apt-get -y install python
```

2. `docker build -f toto -t demo-ubuntu2 .`

3. `docker run -i -t demo-ubuntu2 python --help`

## Packaging java application in a Docker container

TO BE CONTINUED
