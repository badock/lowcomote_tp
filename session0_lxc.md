---
title: Appendix - Demonstration on LXC containers
excerpt: ""
order: 6
---

# Demonstration on LXC containers
1. TOC
{:toc}

## Resources

This demonstration directly uses a subpart of the content of the online try of LXC/LXD at [this link](https://linuxcontainers.org/lxd/try-it/).  Please go to the corresponding link and start the online try.

## Creating a LXC container

1. `lxc image list`
   No images are currently available on the host OS.

2. `lxc launch images:ubuntu/18.04 first`
   This command starts a container called "first" using the Ubuntu 18.04 image.

3. `lxc list`
   Your new container is listed in the list of containers.

4. `lxc image list`
   The ubuntu/18.04 is now listed in the list of known images.

5. `lxc info first`
   Get some info on your container.

## Launch a command on the container

1. `lxc exec first -- apt-get update` and `lxc exec first -- apt-get install sl -y`
   These commands respectively execute the command `apt-get update` and `apt-get install sl -y` in your container. The package `sl` is a well known ASCII train application on Linux. Let's try it!

2. `lxc exec first -- /usr/games/sl`
   Did you see the train? 

### Get a bash on the container

1. `lxc exec first -- bash`
   Now you should see `root@first:~#`. This means that you're having a bash directly on your container!

2. `mkdir test` and `touch test/toto.org` and `ls test/`
   You've just created a directory `test` and a `org` file in it `toto.org`. If you don't know [emacs-org
mode](https://orgmode.org/), take a look! It could be helpful to handle your thesis project in a light way! (bibliography management, daily TODO lists, agenda, reproducible research and much more!)

## Stop the container and restart it

1. `exit` 
   You've quit the bash on your container.

2. `lxc stop first`
   You've stopped your container.

3. `lxc start first`
   You've started it again.

4. `lxc exec first -- /usr/games/sl` and `lxc exec first -- ls test/`
   You've checked that your new package `sl` and your new file are still there!

## Snapshot of the container and restoration

1. `lxc snapshot first clean`
   You've just created a snapshot from your running container first under the name `clean`.

2. `lxc exec first -- rm -Rf /etc /usr`
   Oups! You've just deleted a very important part of the Linux OS!

3. `lxc exec first -- bash`
   Ohhh no! Your bash is broken!

4. `exit`

5. `lxc restore first clean`
   Ok it seems to work to restore the `first` container!

6. `lxc exec first -- bash` and `ls test/`
   YES!
