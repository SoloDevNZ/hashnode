---
title: "Installing Distrobox."
datePublished: Sat Dec 02 2023 09:00:12 GMT+0000 (Coordinated Universal Time)
cuid: clpntp8do000008johtkmh4qa
slug: installing-distrobox
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701641439392/51175fc3-c574-4f9c-95b4-e4d30f92562d.png
tags: containers, distrobox

---

Distrobox is a container system. The main advantages of using Distrobox include:

* The isolation of container libraries,
    
* Easy access to the host hardware, and
    
* Easy access to the host file system.
    

## Prerequisites.

* A Linux-based distro (I use Ubuntu), and
    
* [A Docker installation](https://solodev.app/installing-docker).
    

## Installing Distrobox on Ubuntu.

* I update my `homelab` system:
    

```bash
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt --fix-broken install && sudo apt autoremove -y
```

* I use the Curl command to install Distrobox:
    

```bash
curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/install | sudo sh
```

> NOTE: If I'm nervous about running a foreign script file, I can always download it and manually check its contents.

* I reboot the `homelab` system to load the Distrobox settings into memory.
    

## Uninstalling Distrobox.

* The following Curl command removes Distrobox:
    

```bash
curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/uninstall | sudo sh
```

## Distrobox Environment Commands.

* I use the following to display a list of Distrobox commands:
    

```bash
distrobox
```

## In Conclusion.

Now that I have Distrobox installed, I can run containers that have access to my home directory and hardware devices. I can also run GUI applications that are not packaged as Flatpak or AppImage.

Until next time: Be safe, be kind, be awesome.