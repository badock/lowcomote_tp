---
title: Session 1 - Getting started with a virtual machine
excerpt: ""
order: 1
---


In this Session we will configure a virtual machine based on an image, see it in operation, and we will perform various operations on it.

1. TOC
{:toc}

# Virtual Machine : a quick reminder

![/assets/img/session1/vm.png](/assets/img/session1/vm.png)

A virtual machine is the emulation of a computer system that runs on a real computer. It is based on __virtual disk image file__ that contains the copy of a physical disk. A virtual machine can have one or several network interfaces, that can be mapped on physical network interfaces.

# Getting started with VirtualBox

For this set of sessions we have chosen to use the [VirtualBox](https://www.virtualbox.org/) hypervisor. VirtualBox is an opensource virtual machine hypervisor currently developed by Oracle.

## Installation of VirtualBox

Browse to the [Download virtual box](https://www.virtualbox.org/wiki/Downloads) page, click on the link corresponding to your Operating System and proceed with the installation.


![/assets/img/session1/install1.png](/assets/img/session1/install1.png)
![/assets/img/session1/install2.png](/assets/img/session1/install2.png)
![/assets/img/session1/install3.png](/assets/img/session1/install3.png)

Once installed, launch VirtualBox. You should get something similar to the screenshot below:

[empty VirtualBox](/assets/img/session1/empty_vbox.png)

## Importing a virtual machine with VirtualBox

Ask the teacher for a flash drive that contains an exported version of the virtual machine that will be used for the practical session.

![/assets/img/session1/import1.png](/assets/img/session1/import1.png)

Double click on the __ubuntu_cours_madrid.ova__ file, VirtualBox should display a screen as follow:

![/assets/img/session1/import2.png](/assets/img/session1/import2.png)

Confirm the import of the virtual machine. And wait until the virtual machine is imported.

![/assets/img/session1/import3.png](/assets/img/session1/import3.png)

Once the import is finished, the virtual machine should appear in VirtualBox:

![/assets/img/session1/import4.png](/assets/img/session1/import4.png)


# Common operations with virtual machines

## Configuring the VM network

![/assets/img/session1/configure1.png](/assets/img/session1/configure1.png)

## Start / Restart / Suspend

Click on the __ubuntu_cours_madrid__ virtual machine and then click on the __Start__ button, as in the following screenshot:

![/assets/img/session1/launch1.png](/assets/img/session1/launch1.png)

A new window depicting the output of the virtual machine should appear: the virtual machine is booting up:
![/assets/img/session1/launch2.png](/assets/img/session1/launch2.png)

Once booted, you can interract with the virtual machin as it was a classic computer.

![/assets/img/session1/launch3.png](/assets/img/session1/launch3.png)


## Configure a shared folder between an host and the VM

To ease the practical session, we will now configure a shared folder
between the virtual machine and the physical host. The idea behind a
shared folder is that any file written by a host in the shared folder,
will be seen by the other host.

To create a shared folder, on the preferences of VirtualBox click on
the "Shared Folders" tab and configure a new shared folder as in the
following screenshot:

![/assets/img/session1/shared1.png](/assets/img/session1/shared1.png)

Use the following parameters:

- ***Folder path***: a folder on your computer. Here are valid examples of shared folders:
    + In Linux it may be something like `/home/<username>/shared_folder`.
    + In macOS it may be something like `/Users/<username>/shared_folder`.
    + In windows it may be `C:/shared_folder`.
- ***Folder name***: use the `shared_folder` value
- ***Automount***: select this option to ensure that the shared folder is activated when the virtual machine is run.
- ***Mount Point***: use `/home/lowcomote/workspace`.
