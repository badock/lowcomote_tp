---
title: Session 2 - Getting started with Docker
excerpt: ""
order: 2
---

Docker is a project that enables to run applications in a isolated environments. In this session we will see how docker can be used to run a service.

1. TOC
{:toc}

# Docker : quick reminder

TODO

# Toward running a service with Docker

## Basic operations

First, you can have an overview of available docker commands by just running `docker help` in the terminal:
```bash
[...]
Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  deploy      Deploy a new stack or update an existing stack
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
[...]
```

Now, we will see container in action by deploying in the virtual
machine an instance of Gitlab, a famous opensource project that
provides a web interface to [git](https://en.wikipedia.org/wiki/Git)
(a distributed version-control system).

### Images

Docker images are stored in Images repositories such as [Docker
Hub](https://hub.docker.com). In this Section we will work with a
gitlab image hosted on the [gitlab's section of Docker
hub](https://hub.docker.com/u/gitlab).

First we need to pull a docker image from a repository:
```bash
docker pull gitlab/gitlab-ce
```

It should download the image as in the recording below:

<script id="asciicast-298563" src="https://asciinema.org/a/298563.js" async></script>

Now we can list the images stored on the virtual machine with the `docker image ls` command (**it requires super user privileges**):
```bash
lowcomote@lowcomote-VirtualBox:~$ sudo docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
gitlab/gitlab-ce    latest              13d9da61e07d        5 days ago          1.85GB
```

Please note that there exist many actions on images. You can get a glimpse of these commands by running `docker image help`:
```bash
lowcomote@lowcomote-VirtualBox:~$ docker image help

Usage:	docker image COMMAND

Manage images

Commands:
  build       Build an image from a Dockerfile
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Display detailed information on one or more images
  load        Load an image from a tar archive or STDIN
  ls          List images
  prune       Remove unused images
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rm          Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Run 'docker image COMMAND --help' for more information on a command.
```

### Containers

In this Section we will see how containers can be launched with Docker.

## Running a container based on an image

We can start an instance of the gitlab image by running the following command:
```bash
sudo docker run --detach \
  --publish 8080:80 \
  --name gitlab \
  gitlab/gitlab-ce:latest
```


The complete ID of the newly created container should appear below. In my case, it was:
```bash
3be5d76cb859f7fdcb0bea7e396c7f4d170d92726cfd465630ea39d8014d4a3a
```


To see the running containers, we can run the command `sudo docker ps`: we can notice that the newly created container appears!
```bash
lowcomote@lowcomote-VirtualBox:~/omnibus-gitlab/docker$ sudo docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS                            PORTS                                                               NAMES
3be5d76cb859        gitlab/gitlab-ce:latest   "/assets/wrapper"   8 seconds ago       Up 6 seconds (health: starting)   0.0.0.0:8022->22/tcp, 0.0.0.0:8080->80/tcp, 0.0.0.0:8443->443/tcp   gitlab
```

After few minutes, you should be able to access to gitlab using an URL like `http://<ip_of_the_vm>:8080/` as in the following screenshot:


Now you can delete the container using the following command
```bash
docker kill gitlab
```

It is possible to rerun the container with the following command:
```bash
docker start gitlab
```

To destroy a stopped container, run the following command:
It is possible to rerun the container with the following command:
```bash
docker rm gitlab
```

## Volumes

A Docker container is not persistent: at destruction all its files are lost, which in the case of gitlab means that if you destroy a running instance of gitlab, you may lose all the source codes hosted on gitlab. To handle the case where some data needs to be persistent, Docker provides the `Volumes` mecanism : it maps folders from the physical host to folders of the container, similarly to shared folders with VirtualBox.

Volumes can be specified in the Dockerfile or by passing arguments in the form `--volume host_folder_path:container_folder_path`.

To make the gitlabe container persistent, lets recreate a new gitlab container by specifying volumes:

```bash
sudo docker run --detach \
  --publish 8080:80 \
  --name gitlab \
  --volume /srv/gitlab/config:/etc/gitlab \
  --volume /srv/gitlab/logs:/var/log/gitlab \
  --volume /srv/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

Change the root password in the gitlab welcome screen, destroy the container, and recreate it with the volumes : the data should have been preserved!

Also please note that when the container has been destroyed, the date remains on the physical host:
```bash
lowcomote@lowcomote-VirtualBox:~/omnibus-gitlab/docker$ ls /srv/gitlab/
config  data  logs
```

## Towards building your own docker image

It is interesting to understand how the Gitlab's Docker image is produced. The files required to build the image are located at [https://gitlab.com/gitlab-org/omnibus-gitlab/tree/master/docker](https://gitlab.com/gitlab-org/omnibus-gitlab/tree/master/docker). In this folder we can remark that there is a `Dockerfile` file : this file contains all the instructions to build the docker image.

The `Dockerfile` is the file that contains the instructions that must be run to build an image. In [Session 4](session4_package_and_run.html) we will see how to use `Dockerfile` files to package our own software.
