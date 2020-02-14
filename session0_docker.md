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

A Dockerfile is a file containing a set of instruction to build a new Docker image. Common Dockerfile instructions start with `RUN`, `ENV`, `FROM`, `MAINTAINER`, `ADD`, and `CMD`, among others. They are equivalent to system commands. See [the documentation](https://docs.docker.com/engine/reference/commandline/run/) for more details.

1. create a new Dockerfile `touch your-path/Dockerfile`

2. open it with your favorite text editor (emacs, vi, sublimetext etc.)

3. copy/paste the following content in the Dockerfile

```
FROM ubuntu:latest

RUN apt-get -y update
RUN apt-get install sl -y
```

- The instruction `FROM ubuntu:latest` means that the new Docker image will be built by using ubuntu:latest as a basic layer;
- the other instructions run commands on the basic image before the new layers and image are created, in this example the packages `sl` (remember the train?) is installed.

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
You've actually created another container from the image demo-ubuntu with this command, you have not run the command on the one created before.

2. `docker ps -a`
Now you can see the container you've created so far. Docker has given a name to each container you've created (last column).

3. `docker restart [name]`
You can restart the previous container.

4. `docker attach [name]`
And you can attach to this container to get a bash on it.

5. `python --help`
It should work! But still this illustrates the ephemeral nature of Docker containers.

6. `exit`

8. `docker exec [name] ls`
This executes a command `ls` on the container [name].

9. `docker rm [name]`
You've just removed the container [name].

## Data volume

Let's create another image with an associated volume. Data volumes is the cleanest way to handle persistent data within Docker containers.

1. Create another Dockerfile `toto` with the following content:

```
FROM ubuntu:latest

RUN apt-get -y update
RUN apt-get install sl -y
RUN apt-get -y install python
```

2. `docker build -f toto -t demo-ubuntu2 .`

3. `docker run -i -t demo-ubuntu2 python --help`
Python is installed in this container as the new Dockerfile included it

4. `docker run -it -v /data --name container2 demo-ubuntu2`
You could also use separately `docker volume create [name]` to create a new volume.

5. `ls`
You should see the directory `data` mounted in your container.

6. `touch data/test.org`
You've created a file in data.

7. `exit`

8. `docker inspect container2`
You should see the volume details:
```
"Mounts": [
            {
                "Type": "volume",
                "Name": "a4a840fe25c23c855fcf7f546c4a45b09b1580a39d0203df4630d1d1dd9281b3",
                "Source": "/var/lib/docker/volumes/a4a840fe25c23c855fcf7f546c4a45b09b1580a39d0203df4630d1d1dd9281b3/_data",
                "Destination": "/data",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

9. `docker retsart container2`

10. `docker attach container2`

11. `ls data/`

12. `exit`

13. `docker rm container2`
  You've deleted the container2.

14. `docker volume ls`
The volume is still visible in the list of volumes.
If you add `-v` to the removal of your container, the associated volume would be removed as well.

15. `docker run -it -v [volumne-name]:/app --name container3 demo-ubuntu`
The volume [volume-name] is mounted on the new container3 and placed in the directory `app`.

16. `ls app/`
You should find you previous file `test.org`.



