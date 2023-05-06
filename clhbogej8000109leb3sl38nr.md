---
title: "Docker as a Homelab Service."
seoTitle: "Docker Homelab Service Guide"
seoDescription: "Explore Docker with LXD containers on Ubuntu 22.04 LTS in a Homelab setup. Master creating, configuring, and managing Docker-compatible resources."
datePublished: Sat May 06 2023 07:42:29 GMT+0000 (Coordinated Universal Time)
cuid: clhbogej8000109leb3sl38nr
slug: docker-as-a-homelab-service
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683323432145/461864d8-404e-45da-9ac0-257a637ef69b.png
tags: docker, lxc, lxd, openssh-server

---

## Requirements.

This lab uses the following equipment:

* The `homelab` NUC\* that runs Ubuntu 22.04 LTS, and
    
* The `workstation` PC that runs Ubuntu 22.04 LTS.
    

> NOTE: NUC\* stands for Next Unit of Computing which is nothing more than a compact, small form factor computer.

## An Introduction.

Containers are great [virtualization environments](https://solodev.app/the-magic-of-virtualization) because they isolate running processes from my `homelab` system. After running my tests, I can easily delete any of the containers I used. Linux containers are better than virtual machines because they use fewer resources while running their virtual processes.

In a previous post, I discussed the installation of [the LXD container manager](https://solodev.app/lxd-on-my-homelab) on my `homelab` system. Now, I am using LXD to create a Docker-compatible Linux container and, after deploying the container, I intend to install Docker within it.

Remember: The main purpose of this lab is to assess the viability of *an implementation* that is running within a container.

Now that I know what I want to achieve, it's time to use LXD to spin up a Docker-compatible Linux container.

## Creating an LXC Image for Docker.

> NOTE: Remember, LXC is an abbreviation for LinuX Container.

These are the steps I follow after opening a terminal.

* On the `homelab` system, I initialise an image called `docker` that is built with the `Ubuntu 22.04` image, and then I list all the images and containers that are available on the system. (There should only be one image listed):
    

```bash
$ lxc init ubuntu:22.04 docker
$ lxc ls
```

> NOTE: This `docker` image, when running as a container, will use the same DHCP server, to obtain an IP address, as any other device on my LAN.

> ANOTHER NOTE: Images and containers are THE SAME, but in different states. When starting an image, it changes to being a container. When stopping a container, it changes back to being an image. Again, images are static snapshots while containers are running instances.

The next step is to create an LXD volume.

> **Attribution:**  
> [blog.simos.info](https://blog.simos.info/how-to-get-lxd-containers-obtain-ip-from-the-lan-with-ipvlan-networking/)

## Creating an LXD Volume and Device for Docker.

* On the `homelab` system, I create a volume called `docker-vol`, attach the `docker` container to a (newly created) `docker-hdd` device, and list the volumes within the `docker-pool` storage pool:
    

```bash
$ lxc storage volume create docker-pool docker-vol
$ lxc config device add docker docker-hdd disk pool=docker-pool source=docker-vol path=/var/lib/docker-hdd
$ lxc storage volume list docker-pool
```

Running a container system, like Docker, within another container system (like LXD) will present issues that need addressing.

* I add additional configurations to the `docker` image:
    

```bash
$ lxc config set docker security.nesting true
$ lxc config set docker security.syscalls.intercept.mknod true
$ lxc config set docker security.syscalls.intercept.setxattr true
```

* I check the `docker` configuration:
    

```bash
$ lxc config show docker
```

The next step is to install a Docker instance within the LXC.

> **Attribution:**  
> [ubuntu.com](https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers#2-create-lxd-container)

## Installing Docker within an LXC.

* On the `homelab` system, I start the `docker` image, bash into the `docker` container, and update the container:
    

```bash
$ lxc start docker
$ lxc exec docker bash
# apt clean && apt update && apt dist-upgrade -y && apt autoremove -y
```

* I install the following:
    

```bash
# apt install -y ca-certificates curl gnupg lsb-release
```

* I make a `keyrings` directory, add Docker’s official GPG key, and change the mode of the `docker.gpg` file:
    

```bash
# mkdir -m 0755 -p /etc/apt/keyrings
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
# chmod a+r /etc/apt/keyrings/docker.gpg
```

* I install the Docker repository, update the local repo list, and reboot the container:
    

```bash
# echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# apt update
# reboot
```

* I bash into the `Docker` container, and install the `Docker` instance:
    

```bash
$ lxc exec docker bash
# apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

* I check if the `Docker` instance is running:
    

```bash
# systemctl is-active docker
# service docker status
# status docker
# docker info
```

* I verify the installation:
    

```bash
# docker run hello-world
```

The next step is to increase the Pool and Volume used by the Docker container.

> **Attribution:**  
> [ubuntu.com](https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers#3-install-docker)

## Increasing the Docker Pool and Volume.

* I check the total space of the `docker-pool`, increase the size of `docker-pool` (e.g. from 30GiB to 100GiB), list the volumes connected to `docker-pool`, and increase the size of `docker-vol` to 50GiB:
    

```bash
# lxc storage info docker-pool
# lxc storage set docker-pool 100GiB
# lxc storage volume list docker-pool
# lxc storage volume set docker-pool docker-vol size 50GiB
```

The next step is to resolve any Docker IP address conflicts when running multiple LXCs that include Docker.

## Running Multiple Docker Containers.

If I run multiple containers where each is running Docker, then I will need to alter the Docker IP addresses to avoid any conflicts.

* Within a container that is running a Docker instance, I edit the `docker.service` file:
    

```bash
# sudo nano /lib/systemd/system/docker.service
```

* I add `--bip "172.17.0.?/24"` at the end of the `ExecStart=/usr/bin/dockerd` line, save the changes, and exit Nano.
    

> NOTE: Replace the question mark (?) above with an unused IP number (1-254).

* I reload the daemon, restart Docker, reboot the container, and list the containers to check that each Docker instance (in each container *running* Docker) has a unique IP:
    

```bash
# systemctl daemon-reload
# systemctl start docker
# reboot
$ lxc ls
```

## Adding a User Account within an LXC.

* I bash into the `docker` container (if needed), add a new user account, and add the new user to the `sudo` group:
    

```bash
$ lxc exec docker bash
# adduser brian
# usermod -aG sudo brian
```

Adding the SSH server (along with this user account) will *finally* make the `docker` container accessible across the LAN.

## Installing the OpenSSH Server within an LXC.

* Within the `docker` container, I install the `SSH` server:
    

```bash
# apt install openssh-server
```

* I open the "sshd" file:
    

```bash
# nano /etc/ssh/sshd_config
```

* I edit the following settings, and save the "sshd" file:
    

```plaintext
Port 7822
Protocol 2
PermitRootLogin no
AllowUsers brian
PasswordAuthentication yes
```

> NOTE: Later, I will return to this file and change the PasswordAuthentication setting.

* I test the SSH configuration and reboot the `docker` container:
    

```bash
# sshd -t
# reboot
```

With the SSH Server installed, I can now access the container from any device connected to the same LAN subnet.

## In Conclusion.

This post covered:

* The creation of a Docker-compatible container,
    
* Setting up a storage Volume, and Device, for Docker,
    
* The installation of Docker *within* that container,
    
* The process for changing the Docker IP address,
    
* Adding a user account to the container, and
    
* Installing the SSH Server.
    

LXCs are great virtualization technologies because are way more efficient than virtual machines. For the sake of clarity, I should also point out that Docker containers are way more efficient than LXCs.

I think virtual machines, LXCs, and Docker containers, have their roles to play in modern engineering. The trick lies in knowing which to use in any given moment.