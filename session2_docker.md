---
title: Session 2 - Getting started with Docker
excerpt: ""
order: 2
---

Docker is a project that enables to run applications in a isolated environments. In this session we will see how docker can be used to run a service.

1. TOC
{:toc}

# Docker : quick reminder

With Docker, applications run inside containers which are based on
images. An image is composed of several immutable layers
(i.e. intermediate images). Data inside a container is stored on an
Union file system which stores the difference between the running file
system and the base image. It is possible to specify volumes, which
contains data stored outside the Union file system, on the host's file
system.

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
machine an instance of Gogs, a lightweight opensource project that
provides a web interface to [git](https://en.wikipedia.org/wiki/Git)
(a distributed version-control system).

### Images

Docker images are stored in Images repositories such as [Docker
Hub](https://hub.docker.com). In this Section we will work with an
image og the [Gogs](https://gogs.io/) hosted on the [Gogs' section of
Docker hub](https://hub.docker.com/r/gogs/gogs).

First we need to pull a docker image from a repository:
```bash
sudo docker pull gogs/gogs
```

It should download the image as in the recording below:

<script id="asciicast-d40ZUUJeyD5MkT2bYemHyAllg" data-size="small" data-cols="80" data-rows="30" src="https://asciinema.org/a/d40ZUUJeyD5MkT2bYemHyAllg.js" async></script>

Now we can list the images stored on the virtual machine with the `docker image ls` command (**it requires super user privileges**):
```bash
lowcomote@lowcomote-VirtualBox:~$ sudo docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
gogs/gogs           latest              a14b25251c6c        2 days ago          102MB
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
  --name=gogs \
  -p 8022:22 -p 8080:3000 \
  gogs/gogs
```


The complete ID of the newly created container should appear below. In my case, it was:
```bash
830928e7fceb66ae318372fd96378092b6f8ccad009bcadff47cb3a71c2db311
```


To see the running containers, we can run the command `sudo docker ps`: we can notice that the newly created container appears!
```bash
lowcomote@lowcomote-VirtualBox:~/omnibus-gitlab/docker$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                          NAMES
830928e7fceb        gogs/gogs           "/app/gogs/docker/st…"   7 seconds ago       Up 4 seconds        0.0.0.0:8022->22/tcp, 0.0.0.0:8080->3000/tcp   gogs
```

You can review the progress of the container startup by running `sudo docker logs -f gitlab`

After a few moment, you should be able to access to **Gogs** using an URL like `http://<ip_of_the_vm>:8080/` as in the following screenshot:
![/assets/img/session2/gogs1.png](/assets/img/session2/gogs1.png)

Do the following configuration actions:
- Select `SQLite3` in the **Database Type** parameter
- Put `http://<ip_of_the_vm>:8080/` in the ***Application URL*** parameter
- Unfold the `Admin Account Settings` and decide the credentials of the root account

Proceed with the install. It should result in the following page:
![/assets/img/session2/gogs2.png](/assets/img/session2/gogs2.png)

Now you can delete the container using the following command
```bash
docker kill gogs
```

It is possible to rerun the container with the following command:
```bash
docker start gogs
```

To destroy a stopped container, run the following command:
It is possible to rerun the container with the following command:
```bash
docker rm gogs
```

## Volumes

Data inside a docker container is not meant to be persistent: at destruction all its files are lost, which in the case of **Gogs** means that the git repositories would be lost. To handle the case where some data needs to be persistent, Docker provides `Volumes` which can be used to specify which folders should be excluded from the *Union file system*. Docker volumes are stored on the host file system in the `/var/lib/docker/volumes` folder.

Volumes can be specified in the Dockerfile or by passing arguments in the form `--volume host_folder_path:container_folder_path`. There are two advantages of specifying a local folder for the volume:
- It is easier to access the data from the host
- It is possible to share data between containers
- When a container is destroyed and recreated, the data in the volume will be reused

To make the gitlabe container persistent, lets recreate a new **Gogs** container by specifying volumes:

```bash
sudo docker run --detach \
  --name=gogs \
  -p 8022:22 -p 8080:3000 \
  -v /var/gogs:/data \
  gogs/gogs
```



## Towards building your own docker image

It is interesting to understand how the **Gogs** Docker image is produced. The files required to build the image are located at [https://github.com/gogs/gogs](https://github.com/gogs/gogs). In this folder we can remark that there is a `Dockerfile` file : this file contains all the instructions to build the docker image.

The `Dockerfile` is the file that contains the instructions that must be run to build an image. In [Session 4](session4_package_and_run.html) we will see how to use `Dockerfile` files to package our own software.
