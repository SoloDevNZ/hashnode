---
title: "Installing Docker."
datePublished: Fri Dec 01 2023 09:00:12 GMT+0000 (Coordinated Universal Time)
cuid: clpme9doe000n09l28kuo26cd
slug: installing-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701641423263/e61d68f1-7f92-42ac-b2c0-fe85daad21f7.png
tags: docker, containers, isolation, installation, portability

---

Docker is a container system. The main advantages of using Docker include:

* The isolation of running processes,
    
* The ease of creating new instances,
    
* Fast deployments, and
    
* Fast migrations.
    

## **Prerequisites.**

* A Linux-based distro (I use Ubuntu).
    

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

## In Conclusion.

Now that I have Docker installed, I can quickly deploy and scale applications into any environment and know my code will run.

Until next time: Be safe, be kind, be awesome.