---
layout: post
title:  "How to install Arch Linux"
time: '2018-01-02 02:29:30 -0500'
categories: arch linux
---

## Creating live USB and booting from it
Get yourself a copy of the latest Arch distro from Arch Linux official website [here](https://www.archlinux.org/download/){: target="_blank"}. After downloading the ISO make the bootable USB drive using e.g [Etcher](https://etcher.io/){:target="_blank"}.

Boot from the installation media you've just created and choose **Boot Arch Linux (x86_64)** option from the menu to proceed with the installation. Once you system is booted you'll be automatically logged in as a root user.

## Partitioning the disk(s)
To check your current existing drives run `fdisk -l`. In the list find a name of the drive you want to install your system to and remember it (something like */dev/sda*).
For partitioning purposes we'll be using `cfdisk` command.


## Installing display manager
```bash
sudo pacman -S gdm  #to install gnome display manager
sudo systemctl enable gdm.service  #to enable display manager
sudo systemctl start gdm #to start display manager 
```

