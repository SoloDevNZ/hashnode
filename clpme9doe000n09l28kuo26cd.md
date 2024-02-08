---
title: "Installing Docker."
datePublished: Fri Dec 01 2023 09:00:12 GMT+0000 (Coordinated Universal Time)
cuid: clpme9doe000n09l28kuo26cd
slug: installing-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701641423263/e61d68f1-7f92-42ac-b2c0-fe85daad21f7.png
tags: docker, containers, isolation, installation, portability

---

Update: Thursday 8<sup>th</sup> February 2024.

# TL;DR.

This post explains how to install Docker, a tool that makes it easy to develop, ship, and run apps. Docker puts an app's code, infrastructure, libraries, and dependencies into one small package that runs on any computer. To install Docker, I update my system, install any requirements, add Docker's official GPG key, install the Docker repository, and check the installation. This post also shows how to uninstall Docker. The main advantages of using Docker include:

* An ability to run my apps on any system,
    
* Isolating processes from other containers and the host,
    
* Quick assembly and deployment, and
    
* Easily making containers for my apps.
    

> **Attributions:**
> 
> [https://www.docker.com/](https://www.docker.com/)***↗***

# An Introduction.

Docker is a utility that helps me package my applications so they can run on any computer system.

> The purpose of this post is to present a process for installing, and using, Docker.

# The Big Picture.

Docker is a container system. The main advantages of using Docker include:

* The isolation of running processes,
    
* The ease of creating new containers,
    
* Fast deployments, and
    
* Fast migrations.
    

# **Prerequisites.**

* A Linux-based distro (I use Ubuntu).
    

# What is Docker?

Docker is a free tool that helps me develop, ship, and run my applications in containers. Containers are lightweight bundles that package my application source code, infrastructure, libraries, and dependencies into one file that can run on any PC.

## Installing Docker.

* I update my `homelab` system:
    

```bash
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt --fix-broken install && sudo apt autoremove -y
```

* I install the following requirements:
    

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

* I make a `keyrings` directory:
    

```bash
mkdir -m 0755 -p /etc/apt/keyrings
```

* I add Docker’s official GPG key:
    

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

* I change the mode of the `docker.gpg` file:
    

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

* I install the `Docker` repository:
    

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

* I update the `homelab` repo list:
    

```bash
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt --fix-broken install && sudo apt autoremove -y
```

* I install `Docker` and its requirements:
    

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

* I check if `Docker` is active:
    

```bash
systemctl is-active docker
```

* I check the status of `Docker`:
    

```bash
service docker status
```

* I check the `Docker` version:
    

```bash
sudo docker version
```

* I list the information for Docker:
    

```bash
sudo docker info
```

* I create the Docker group, if required:
    

```bash
sudo groupadd docker
```

* I add my user account to the Docker group:
    

```bash
sudo usermod -aG docker ${USER}
```

* I verify the installation:
    

```plaintext
sudo docker run hello-world
```

* I reboot the `homelab` system to load the Docker settings into memory (or I can reboot the system after installing Distrobox.)
    

## Uninstalling Docker.

* This command uninstalls Docker:
    

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt remove $pkg; done
```

# The Results.

Docker is an invaluable tool that allows me to package my applications into containers, ensuring that they can run on any system. This simplifies the process of developing, shipping, and running my applications, and also enhances the efficiency of deployments and migrations. After going through the installation process, I should now have Docker installed on my Linux-based system, and be able to leverage its capabilities to improve my development workflow. Remember, the power of Docker lies in its ability to isolate running processes and easily create new containers, improving the way I deploy my applications.

# In Conclusion.

How do I package my applications so they can run on ANY computer?

This was Docker, a container system that isolated any running processes, easily created new containers, supported fast app deployments, and allowed quick and easy migrations. It was like having my own, personal mini-shipyard for "shipping" applications to... anywhere.

How did it work?

Docker packaged the source code for my app, its infrastructure, any required libraries, and all the needed dependencies into one lightweight bundle. This bundle, or 'container', could then run on any PC. It was like having my entire tech stack in a portable suitcase!

This guide walked me through installing Docker.

First, I needed a Linux-based distro. I use Ubuntu. Then followed these steps:

1. I updated my system.
    
2. I installed the necessary requirements.
    
3. I made a 'keyrings' directory.
    
4. I added Docker’s official GPG key.
    
5. I changed the mode of the 'docker.gpg' file.
    
6. I installed the Docker repository.
    
7. I updated the repo list.
    
8. I installed Docker and its requirements.
    
9. I checked if Docker was active.
    
10. I verified the installation.
    
11. I rebooted the system to load the Docker settings into memory.
    

Voila! I installed Docker.

But what if I wanted to uninstall Docker? Just one command does the trick. Easy-peasy, right?

Docker is an invaluable tool that simplifies the process of developing, shipping, and running applications. It's a game-changer in the world of deployments and migrations.

So, who's ready to give Docker a try? Or if you're already using it, share your experience. How has Docker improved your workflow?

Until next time: Be safe, be kind, be awesome.