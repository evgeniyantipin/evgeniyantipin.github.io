---
layout: post
title:  "How to install Arch Linux"
time: '2018-01-02 02:29:30 -0500'
categories: arch linux
---

## Creating live USB and booting from it
Get yourself a copy of the latest Arch distro from Arch Linux official website [here](https://www.archlinux.org/download/){: target="_blank"}. After downloading the ISO make the bootable USB drive using e.g [Etcher](https://etcher.io/){: target="_blank"}.

Boot from the installation media you've just created and choose **Boot Arch Linux (x86_64)** option from the menu to proceed with the installation. Once you system is booted you'll be automatically logged in as a root user.

## Partitioning and formatting the disk(s)
To check your current existing drives run `fdisk -l`. In the list find a name of the drive you want to install your system to and remember it (something like */dev/sda*). For partitioning purposes we'll be using `cfdisk` command. Type in `cfdisk /dev/sda` into the terminal and select **dos** to create new partitioning table. I'm gonna create three separate partitions for my system: one to use as a EFI system partition, second for swap and third for the system itself. (*/boot*, *swap* and */*). Format your primary partition using `mkfs.ext4 /dev/sda2` command.

## Installing the system
```bash
mount /dev/sda2 /mnt
mkswap /dev/sda1
swapon /dev/sda1
```
Installing base system to our mounted /sda2 partition
```bash
pacstrap /mnt base base-devel
```
```bash
arch-chroot /mnt
```
Set up the password by typing `passwd`.
Open *locale.gen* file using nano and uncomment locale that you gonna use(*en_US.UTF-8*)
```bash
nano /etc/locale.gen
```
Then save the file and run the command `locale-gen` to generate the chosen locale(s).

Set a time zone (in my case it's Eastern US time):
```bash
rm -rf /etc/localtime
ln -s /usr/share/zoneinfo/US/Eastern /etc/localtime
```
Set a hostname for you computer: `echo whatever > /etc/hostname`

Download the bootloader (GRUB in this case but chose what you like) `pacman -S grub-bios`

And install it on your drive (not the separate partition) `grub-install /dev/sda`

Create an init file, it store info about your hardware that Linux gonna use `mkinitcpio -p linux`

Generate a config file for GRUB `grub-mkconfig -o /boot/grub/grub.cfg`

Exit chroot by typing `exit`

Generate an fstab file that allows Arch to identify your partitions `genfstab /mnt >> /mnt/etc/fstab`

Finally unmount and reboot
```bash
umount /mnt
reboot
```

Log in into your newly installed Arch system by typing your username (*root* in our case) and password that you've created earlier.

```bash
dhcpcd # to initialize internet connection
pacman -S xorg-server xorg-xinit xorg-twm xterm
pacman -S xorg-xclock xorg-apps
pacman -S gnome
```

## Installing display manager
```bash
sudo systemctl enable gdm.service  #to enable display manager
sudo systemctl start gdm #to start display manager 
```

