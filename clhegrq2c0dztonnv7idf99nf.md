---
title: "Docker as a Homelab Service."
seoTitle: "Docker in LXC Guide"
seoDescription: "Explore Docker in a Homelab using LXD for Linux containers. Install Docker, manage IPs, add users, and set up SSH access."
datePublished: Mon May 08 2023 06:30:39 GMT+0000 (Coordinated Universal Time)
cuid: clhegrq2c0dztonnv7idf99nf
slug: docker-as-a-homelab-service
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683406464118/155cac78-6bf4-47dd-af3c-92fc65d34a83.png
tags: docker, lxc, virtualization, lxd, openssh-server

---

## Requirements.

This lab uses the following equipment:

* The `homelab` NUC\* that runs Ubuntu Desktop 22.04 LTS, and
    
* The `workstation` PC that runs Ubuntu Desktop 22.04 LTS.
    

> NOTE: NUC\* stands for Next Unit of Computing which is nothing more than a compact, small form factor computer.

## An Introduction.

Containers are great [virtualization environments](https://solodev.app/the-magic-of-virtualization) because they isolate running processes from my `homelab` system. After running my tests, I can easily delete any of the containers I used. LinuX Containers (LXCs) are better than virtual machines because they use fewer resources while running their virtual processes.

In a previous post, I discussed installing [the LXD container manager](https://solodev.app/lxd-on-my-homelab) on my `homelab` system. This time, I'm going to *use* the LXD to create a Docker-compatible LinuX Container and, after deploying the container, I'll even install Docker inside.

I need to remember: The main purpose of this lab is to assess the viability of *an implementation* that is running within a container.

Now that I know what I want to achieve, it's time to use LXD to spin up that Docker-compatible LinuX Container.

## Creating an LXC Image for Docker.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683422567413/4b5f131b-ba87-4ffc-ad3f-0c5526903bc4.png align="center")

> NOTE: Remember, LXC is an abbreviation for LinuX Container.

These are the steps I follow after opening a terminal.

* On the `homelab` system, I initialise an image called `docker` that is built with the `Ubuntu 22.04` image, and then I list all the images and containers that are available on the system. (There should only be one image listed (it's an image because the STATE is STOPPED)):
    

```bash
$ lxc init ubuntu:22.04 docker
$ lxc ls
```

> NOTE: Images and containers are THE SAME, but exist in different states. When starting an image, it changes to being a container. When stopping a container, it changes back to being an image. Again, images are static snapshots while containers are running instances.

The next step is to create storage for the container.

> **Attribution:**  
> [blog.simos.info](https://blog.simos.info/how-to-get-lxd-containers-obtain-ip-from-the-lan-with-ipvlan-networking/)

## Creating Storage for the Container.

![](https://images.pexels.com/photos/117729/pexels-photo-117729.jpeg?cs=srgb&dl=pexels-azamat-esenaliev-117729.jpg&fm=jpg&w=640&h=427 align="center")

* On the `homelab` system, I create a storage pool called `docker-pool` (that uses `btrfs`), create a volume called `docker-vol` within `docker-pool`, attach the `docker` container to a (newly created) `docker-hdd` device, and list the volumes within the `docker-pool` storage pool:
    

```bash
$ lxc storage create docker-pool btrfs
$ lxc storage volume create docker-pool docker-vol
$ lxc config device add docker docker-hdd disk pool=docker-pool source=docker-vol path=/var/lib/docker-hdd
$ lxc storage volume list docker-pool
```

Here are my results:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683513441704/77b323c0-2721-402e-b507-aba01827b508.png align="center")

The next step is to tweak the container (image) for Docker.

## Tweaking the Container (Image) for Docker.

![](https://images.pexels.com/photos/5584158/pexels-photo-5584158.jpeg?cs=srgb&dl=pexels-jessica-lewis-creative-5584158.jpg&fm=jpg&w=640&h=480 align="center")

Running a container system, like Docker, within another container system, like LXD, will cause issues that need addressing.

* I add additional configurations to the `docker` image:
    

```bash
$ lxc config set docker security.nesting true
$ lxc config set docker security.syscalls.intercept.mknod true
$ lxc config set docker security.syscalls.intercept.setxattr true
```

* I check the `docker` configuration to ensure these settings are in:
    

```bash
$ lxc config show docker
```

The next step is to install a Docker instance within the container.

> **Attribution:**  
> [ubuntu.com](https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers#2-create-lxd-container)

## Installing Docker within the Container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683426169718/1b9050e0-b618-4d84-ad48-5b793705cf85.png align="center")

* On the `homelab` system, I start the `docker` image, bash into the `docker` container, and update the container:
    

```plaintext
$ lxc start docker
$ lxc exec docker bash
# apt clean && apt update && apt dist-upgrade -y && apt autoremove -y
```

* I install the following:
    

```plaintext
# apt install -y ca-certificates curl gnupg lsb-release
```

* I make a `keyrings` directory, add Docker’s official GPG key, and change the mode of the `docker.gpg` file:
    

```plaintext
# mkdir -m 0755 -p /etc/apt/keyrings
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
# chmod a+r /etc/apt/keyrings/docker.gpg
```

* I install the Docker repository, update the local repo list, and exit the container:
    

```plaintext
# echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# apt update
# exit
```

* I bash into the `Docker` container, and install the `Docker` instance:
    

```plaintext
$ lxc exec docker bash
# apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

* I check if the `Docker` instance is running:
    

```plaintext
# systemctl is-active docker
# service docker status
# docker info
```

Here are my results (and `docker info` produce expected outcomes, too):

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683515578740/0b435223-0ae5-4d4d-a544-cb0a9881a0fd.png align="center")

* I verify the installation:
    

```plaintext
# docker run hello-world
```

And again, here are my results:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683516064458/c54800c8-bebb-4da7-982e-b620223949a5.png align="center")

The next step is to increase the size of the Pool and Volume used by the container.

> **Attribution:**  
> [ubuntu.com](https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers#3-install-docker)

## Increasing the Size of the Docker Pool and Volume.

![](https://images.pexels.com/photos/8657665/pexels-photo-8657665.jpeg?cs=srgb&dl=pexels-mo-eid-8657665.jpg&fm=jpg&w=640&h=360 align="center")

* From the `homelab` terminal, I stop the docker container, check the total space of the `docker-pool`, increase the size of `docker-pool` (e.g. from 30GiB to 100GiB), and check the total space of the `docker-pool` again:
    

```plaintext
$ lxc stop docker
$ lxc storage info docker-pool
$ lxc storage set docker-pool size 100GB
$ lxc storage info docker-pool
$ lxc storage get docker-pool size
```

Here are my results:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683518021778/38954c43-eadc-4214-b39a-e929d6c698a4.png align="center")

* From the homelab terminal, I list the volumes connected to `docker-pool`, and increase the size of `docker-vol` to 50GiB:
    

```plaintext
$ lxc storage volume show docker-pool docker-vol
$ lxc storage volume set docker-pool docker-vol size 50GB
$ lxc storage volume show docker-pool docker-vol
```

Here are my results:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683520887236/db1ea706-e206-4b78-b669-72539926783a.png align="center")

The next step is to resolve any Docker IP address conflicts when running multiple LXCs that include Docker.

## Running Multiple Docker Containers.

![](https://images.pexels.com/photos/5859564/pexels-photo-5859564.jpeg?cs=srgb&dl=pexels-charles-parker-5859564.jpg&fm=jpg&w=640&h=960 align="center")

If I deploy multiple containers where they are running their own Docker instances, then I may need to manually alter the Docker IP addresses to avoid any conflicts.

* From the `homelab` terminal, I start the `docker` container, `ls` (list) the containers (because I want the IP addresses), and bash into the container:
    

```plaintext
$ lxc start docker
$ lxc ls
$ lxc exec docker bash
```

Here are my results:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1683522136767/be03b445-0b54-43fc-9272-05d4578d170d.png align="center")

I have two IPV4 addresses. The eth0 host interface got an IP address from the DHCP server on my LAN. The other IP address was assigned by Docker. If I duplicate the `Docker` container, then I *may* need to fix any Docker IP conflicts.

* I open the `docker.service` file in the Nano text editor:
    

```plaintext
# sudo nano /lib/systemd/system/docker.service
```

* I add `--bip "172.17.0.`<mark>?</mark>`/24"` between `ExecStart=/usr/bin/dockerd` and `-H fd:// --containerd=/run/containerd/containerd.sock`, save the changes, and exit Nano.
    

> NOTE: Replace the question mark (<mark>?</mark>) above with a unique IP number (1-254).

* I reload the daemon, restart Docker, exit the container, and list the containers to check that each Docker instance (in each Docker container) has a unique IP:
    

```plaintext
# systemctl daemon-reload
# systemctl restart docker
# exit
$ lxc ls
```

The next step is to add a user account to the container.

## Adding a User Account to the Docker Container.

![](https://images.pexels.com/photos/8761744/pexels-photo-8761744.jpeg?cs=srgb&dl=pexels-pavel-danilyuk-8761744.jpg&fm=jpg&w=640&h=427 align="center")

* From the `homelab` terminal, I bash into the `docker` container, add a new user, and add the new user to the `sudo` group:
    

```plaintext
$ lxc exec docker bash
# adduser brian
# usermod -aG sudo brian
```

And finally, the last step is to install OpenSSH within the container.

## Installing the OpenSSH Server within the Docker Container.

![](https://images.pexels.com/photos/39624/padlock-lock-chain-key-39624.jpeg?cs=srgb&dl=pexels-pixabay-39624.jpg&fm=jpg&w=640&h=427 align="center")

I can use OpenSSH to connect to the container from any device on my LAN.

* From the `homelab` terminal, I bash into the `docker` container and install the OpenSSH server:
    

```plaintext
$ lxc exec docker bash
# apt install openssh-server
```

* I open the "sshd" file:
    

```plaintext
# nano /etc/ssh/sshd_config
```

* I edit the following settings, and save the "sshd" file:
    

```plaintext
Port 22
Protocol 2
PermitRootLogin no
AllowUsers brian
PasswordAuthentication yes
```

> NOTE: In a later post, I will change the PasswordAuthentication setting to 'no'. I also use a FREE port number other than the default `22` setting.

* I test the SSH configuration and reboot the `docker` container:
    

```plaintext
# sshd -t
# reboot
```

And that's it.

## In Conclusion.

This post covered:

* The creation of a Docker-compatible container,
    
* Setting up a storage Volume, and Device, for Docker,
    
* The installation of Docker *within* that container,
    
* The process for changing the Docker IP address,
    
* Adding a user account to the container, and
    
* Installing the SSH Server.
    

LXCs are great virtualization technologies because they are way more efficient than virtual machines. For the sake of clarity, I should also point out that Docker containers are way more efficient than LXCs. Of course, unlike virtual machines and LXCs, Docker containers don't run operating systems.

I think virtual machines, LXCs, and Docker containers, have their roles to play in modern engineering. The trick lies in knowing which to use at any given moment.

Until next time: Be safe, be kind, be awesome.