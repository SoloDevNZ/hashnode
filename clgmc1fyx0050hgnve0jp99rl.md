---
title: "LXD on my Homelab."
datePublished: Tue Apr 18 2023 14:00:41 GMT+0000 (Coordinated Universal Time)
cuid: clgmc1fyx0050hgnve0jp99rl
slug: lxd-on-my-homelab
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682598081001/ba4e7d16-f8c8-46a2-8dea-736804286e10.png
tags: lxc, fail2ban, homelab, ufw, lxd

---

---

## Requirements.

This lab uses the following equipment:

* The `homelab` NUC\* system running Ubuntu 22.04 LTS,
    
* The `Snap` package management system,
    
* An empty storage partition, and
    
* The LXD installation package.
    

> NOTE: NUC\* stands for Next Unit of Computing which is nothing more than a compact, small form factor computer.

## An Introduction.

![containers](https://images.pexels.com/photos/906494/pexels-photo-906494.jpeg?cs=srgb&dl=pexels-chanaka-906494.jpg&fm=jpg&w=640&h=420 align="left")

My previous post described [*why* I'm building a homelab](https://solodev.app/my-first-homelab-a-brief-introduction). This time, I'm going to - partly - describe *how* I'm building a homelab.

My focus throughout this post is to describe how I download and deploy the [LXD container manager](https://ubuntu.com/lxd) onto my `homelab` system.

The LXD is what I use to create *some* of my containers.

> NOTE: Actually, there are *TWO* container technologies I want to use at my startup:
> 
> * The LXD (which I'll explain in a moment), and
>     
> * Docker (which I'll cover over the following *TWO* posts.)
>     

LXD stands for LinuX Daemon and is pronounced: "lexdee". This is a container manager that runs as a background process. It is a tool that helps me manage the containers that I "spin up", "tear down", and use.

LXC stands for LinuX Container and is pronounced: "lexsee". These are operating system-level virtualization instances that can run concurrently on a single host. These containers use the Linux kernel of the host system, as well as other OS resources, to reduce the overhead of running multiple, isolated occurrences.

> NOTE: I can also spin up a virtual machine by using the `--vm` flag. When they're initialised, virtual machines download a different kernel so it can be used *within* the container itself. Virtual machines are useful when I need functionality that is not supported by the host kernel, or when I want to run a completely different operating system.

The first step is to enable access to the `homelab` system from the `workstation` system while blocking *EVERY OTHER SYSTEM IN THE WORLD*.

## Enabling, and Setting Up, UFW.

![firewall](https://images.pexels.com/photos/241028/pexels-photo-241028.jpeg?cs=srgb&dl=pexels-pew-nguyen-241028.jpg&fm=jpg&w=640&h=427 align="left")

The Uncomplicated FireWall is a wrapper utility that makes setting up and using iptables very simple. It is easy to learn and extremely user-friendly. UFW is the first utility, in a toolbox of devices, that is used to "harden" a system. Hardening is the process of reducing an "attack surface", and blocking potential "vectors", that malicious actors *could exploit* to execute their virulent activities. In other words, I will use *any* tools, and change *any* settings, to minimise the chances of "black hats", cyber-criminals, and/or foreign states, using my systems to harm others.

* On the `homelab` system, I enable the UFW, install a UFW rule, and check the status of the UFW:
    

```bash
$ sudo ufw enable
$ sudo ufw allow from 192.168.188.41
$ sudo ufw status
```

> NOTE: UFW will, by default, block all incoming traffic, including SSH and HTTP.

> ANOTHER NOTE: I will update the UFW rules as I deploy other services to the `homelab`.

Another tool for hardening a system is Fail2Ban.

> **Attribution:**  
> [digitalocean.com](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)

## Installing, and Setting Up, Fail2Ban.

![stop](https://images.pexels.com/photos/9166266/pexels-photo-9166266.jpeg?cs=srgb&dl=pexels-jan-van-der-wolf-9166266.jpg&fm=jpg&w=640&h=427 align="left")

Fail2Ban protects Linux systems against many security threats, such as dictionary, DoS, DDoS, and brute-force attacks.

* On the `homelab` system, I install Fail2Ban, then create the `jail.local` file:
    

```bash
$ sudo apt install fail2ban
$ sudo nano /etc/fail2ban/jail.local
```

* I add the following to the `jail.local` file, save the changes, and exit the Nano editor:
    

```plaintext
[DEFAULT]
destemail = your@email.here
sendername = Fail2Ban

[sshd]
enabled = true
port = 22

[sshd-ddos]
enabled = true
port = 22
```

* Finally, I restart Fail2Ban:
    

```plaintext
$ sudo systemctl restart fail2ban
```

The next step is to install the LXD manager which handles the containers, images, profiles, commands, and resources.

## Installing the LXD Manager.

![manager](https://images.pexels.com/photos/1117211/pexels-photo-1117211.jpeg?cs=srgb&dl=pexels-david-dibert-1117211.jpg&fm=jpg&w=640&h=427 align="left")

> NOTE: Remember, LXD is an abbreviation for \[L\]inu\[X\] \[D\]aemon.

* On the `homelab` system, I run the updates and upgrades, install Snap, and install `LXD`:
    

```bash
$ sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove -y
$ sudo apt install snapd
$ sudo snap install lxd
```

* I'll also add my account to the LXD group if needed:
    

```bash
$ sudo adduser brian lxd
```

> NOTE: Adding myself to the LXD group means I can skip the `sudo` requirement when issuing LXD and LXC commands.

The next step, although not necessary, is to set up an empty drive (or partition) where I can store my images.

## OPTIONAL: Preparing an Empty Drive (or Partition) for LXD Storage.

![HDD](https://images.pexels.com/photos/33278/disc-reader-reading-arm-hard-drive.jpg?cs=srgb&dl=pexels-pixabay-33278.jpg&fm=jpg&w=640&h=427 align="left")

* Using an empty drive (or partition) to store my images has advantages. I can:
    
    * Secure images with repeatable, scheduled backups (think: CRON Jobs),
        
    * Rebuild the host system without losing images,
        
    * Move the drive (or partition) to another system,
        
    * Improve reliability, performance, or capacity,
        
        * Enable the cluster option, and/or
            
        * Include more drives.
            
* On the `homelab` system, I use Gnome Disks to format an empty partition without installing a filesystem.
    
* In a terminal, I list the installed devices, unmount an empty partition, and prepare the partition for use:
    

```bash
$ sudo lshw -short
$ sudo umount /dev/sda2
$ sudo dd if=/dev/zero of=/dev/sda2 bs=4M count=10
```

> NOTE: In this example, `/dev/sda2` is the partition device I've decided to use.

The next step is to initialise the LXD for use.

## Initialising the LXD Manager.

![initial](https://images.pexels.com/photos/10394994/pexels-photo-10394994.jpeg?cs=srgb&dl=pexels-brett-jordan-10394994.jpg&fm=jpg&w=640&h=480 align="left")

* On the `homelab` the system, I initialise the LXD service and *mostly* use the default settings during setup:
    

```bash
$ lxd init
```

These are my settings when I initialise the LXD service: I "like to use an existing empty block device" (as mentioned above)

* ...where the "Path" is `/dev/sda2`,
    
* I don't use a "local network bridge"
    
* ...but I do use a "host interface"
    
* ...called `eno1`.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683418968185/e2b4eea4-1e46-4bd4-88bc-383d9102d9de.png align="center")

* Optionally, I install `zfsutils-linux` if I choose to use the zfs storage driver (where zfs is the default option during the LXD initialisation):
    

```bash
$ sudo apt install -y zfsutils-linux
```

The last step is to test the LXC commands by spinning up and tearing down a couple of containers.

## Running a Quick Test.

![test](https://images.pexels.com/photos/60022/microscope-slide-research-close-up-60022.jpeg?cs=srgb&dl=pexels-public-domain-pictures-60022.jpg&fm=jpg&w=640&h=424 align="left")

From the `homelab` system, I initialise an image called `test1`, launch a container called `test2`, list the images and containers, (try to) delete `test1` and `test2`, list the containers again only to see that `test2` still exists, delete `test2` using the -f(orce) flag, and run the list command to show that all the containers and images have been deleted:

```bash
$ lxc init ubuntu:22.04 test1
$ lxc launch ubuntu:22.04 test2
$ lxc ls
$ lxc delete test1 test2
$ lxc ls
$ lxc delete test2 -f
$ lxc ls
```

> NOTE: While the `test1` instance was NOT running, it was called an image. If I told it to START, then it would have become the `test1` container. The `test2` instance WAS running, so it was called a container. If I told it to STOP, then it would have become the `test2` image. Yet another example of useless trivia floating around in my brain.

## In Conclusion.

![an image of plastic tiles with letters that spell conclusion](https://images.pexels.com/photos/1888005/pexels-photo-1888005.jpeg?cs=srgb&dl=pexels-ann-h-1888005.jpg&fm=jpg&w=640&h=427 align="left")

This blog covered:

* The initialisation, and use, of the UFW,
    
* The installation, and use, of Fail2Ban,
    
* The preparation of an empty partition,
    
* The installation of the LXD manager,
    
* The initialisation of the LXD, and
    
* A quick test of the LXC commands.
    

LXD is a great system for spinning up, and running, multiple LXCs on a single host. (I hate it when my computers become a cesspool of development debris and database detritus, so using disposable containers for experimental exercises is a great use case for me.) Docker is *another* container system that is *extremely* popular with developers.

Be sure to check out my next post in this series where I run a Docker service within an LXD container.