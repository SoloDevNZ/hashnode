---
title: "Installing LXD and Using LXCs."
datePublished: Fri Jan 19 2024 09:00:13 GMT+0000 (Coordinated Universal Time)
cuid: clrkeu4zt000o09l80kqzbpv8
slug: installing-lxd-and-using-lxcs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704964017492/7fc82ddb-0d11-4faa-80ec-dfa4ec46a693.png
tags: lxc, containers, virtual-machines, lxd

---

# TL;DR.

LXD (<mark>L</mark>inu<mark>X</mark> <mark>D</mark>aemon) is a container manager for creating and managing containers. LXCs (<mark>L</mark>inu<mark>X</mark> <mark>C</mark>ontainers) are isolated system instances where anything within the container can NOT affect other containers or the base distro/OS. Also, multiple container instances can run concurrently on a single host.

> **Attributions:**
> 
> [https://ubuntu.com/lxd](https://ubuntu.com/lxd) ***↗.***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973638230/7de81bc6-cc05-4948-9bee-df36ca3ca67f.png align="center")

# An Introduction.

This is a concise (read: less rambling) version of [a post from 2023](https://solodev.app/2-of-10-lxd-on-the-homelab). This time around, I will be more specific about my intentions for this adapted chronicle.

> The purpose of this post is to present the installation, and likely uses, of LXD/LXC.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973659623/47b64773-c25d-40ae-a47d-642b09f48778.png align="center")

# The Big Picture.

Containers are considered the Second Wave of System Isolation Technologies. The First Wave were virtual machines and the Third Wave are WASM/WASI binaries. Each Wave brought their own strengths and weaknesses.

| Name | Advantage | Disadvantage |
| --- | --- | --- |
| Wave 1: Virtual Machines | Isolation, less hardware, systems isolation, security, multiple OSs, ISA. | Resource heavy, expensive, complex. |
| Wave 2: Containers | Isolation, reduced complexity, security, distributed computing, consistency, orchestration, portability, efficiency, scalability, automation, shared kernel. | Stateless, may be hard to network, compatibility with other container technologies, |
| Wave 3: WASM | Portable, assemblies not needed after compilation, can run outside the browser with WASI. | Download size on Edge/2G and GSM/3G, not aware of the DOM - yet, lacks standard security defences. |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973687948/bc3fcb37-a70f-44ad-92a4-6805409a0c44.png align="center")

# Prerequisites.

* A Linux-based distro (I use Ubuntu).
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973704880/d291b34b-927a-4873-8ad3-b3bf39b53771.png align="center")

# Installing the Snap Package Manager.

* I update and upgrade my system:
    

```plaintext
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt --fix-broken install && sudo apt autoremove -y
```

* I install the Snap package manager:
    

```plaintext
sudo apt install snapd
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973721961/f524db87-84e9-4877-9daa-9c84c7c33f07.png align="center")

# Using Snap to Install the LXD Container Manager.

* I use the Snap package manager to install the LXD container manager:
    

```plaintext
sudo snap install lxd
```

* I add my account to the LXD group (to avoid `sudo` when using LXD/LXC):
    

```plaintext
sudo adduser brian lxd
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973740417/c6baee44-5816-42fb-82ae-03f0a2042cf9.png align="center")

# Initialising the LXD Container Manager.

* I list the host interfaces:
    

```plaintext
ip link show
```

> NOTE: These are my results when I display my host interfaces.
> 
> ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704966803709/e7f3f444-f0d7-461a-9627-05788179fa06.png align="center")
> 
> I will use the "enp6s0" host interface when `init`ialising the LXD service.

* I `init`ialise the LXD service and *mostly* use the default settings during setup:
    

```plaintext
lxd init
```

> NOTE: These are my settings when I initialise the LXD service:
> 
> ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704966903788/7965e5a7-ccbc-41a5-9779-9fb273a6c74f.png align="center")
> 
> Notice my deviations in the above screen grab:
> 
> * Q9 where I answer "no" to a local network bridge,
>     
> * Q10 where I answer "yes" to using a host interface, and
>     
> * Q11 where I answer "enp6s0" as the name of the interface.
>     
> 
> With these settings, my router will assign IP addresses to any containers I launch.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973824000/1d3972b9-fdb6-4b2a-b634-796794b1dd07.png align="center")

# Running a Quick Test.

* I `init`ialise an image called `test1`:
    

```plaintext
lxc init ubuntu:22.04 test1
```

* I `launch` a container called `test2`:
    

```plaintext
lxc launch ubuntu:22.04 test2
```

* I `list` the images and containers:
    

```plaintext
lxc ls
```

> NOTE: `lxc init` creates a static image while `lxc launch` also creates a static image but then runs it as a dynamic container.

* I (try to) delete `test1` and `test2`:
    

```plaintext
lxc delete test1 test2
```

* I list the containers again only to see that `test2` still exists:
    

```plaintext
lxc ls
```

* I delete `test2` using the -f(orce) flag:
    

```plaintext
lxc delete test2 -f
```

* I list the containers one more time to see everything has been deleted:
    

```plaintext
lxc ls
```

> NOTE: Images can be deleted because they are real files, but a container is an ephemeral instance of an image. Containers are not deleted, they are stopped. Once the container is stopped, I can delete it's image. Or I can -(f)orce the issue, as above, which stops the container and deletes the image in one command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973846712/332bdfb7-d6d2-4896-bc7f-ea1a43428cee.png align="center")

## Using Snap to Uninstall the LXD Container Manager.

* I use the Snap package manager to uninstall the LXD container manager:
    

```plaintext
sudo snap remove --purge lxd
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973867211/6aa0899d-e834-451f-8a15-b92af966eb7e.png align="center")

# 23 Common Commands.

Here is a list of 23 commands that maybe somewhat useful. Although I commonly use only a handful of commands from this list, it's nice to have a reference on file:

1. `lxc --version` to check the version (and if the command doesn't work, then I'd know an installation problem occurred),
    
2. `lxc network list` to list all the network adapters,
    
3. `lxc init ubuntu:22.04 test-container3` to download an Ubuntu container,
    
4. `lxc launch ubuntu:22.04 test-container3` to launch an Ubuntu container,
    
5. `lxc storage ls` to list storage pool,
    
6. `lxc list` or `lxc ls` to list all the images and containers,
    
7. `lxc stop test-container3` to stop a container,
    
8. `lxc start test-container3` to start a container,
    
9. `lxc restart test-container3` to restart a container,
    
10. `lxc stop test-container3` followed by `lxc delete test-container3`, or `lxc stop test-container3 -f` to delete a container,
    
11. `lxc exec test-container3 cat /etc/os-release` to execute a command on the container,
    
12. `lxc info test-container3` to check a container's Information,
    
13. `lxc exec test-container3 -- bash` to get root access to a container,
    
14. `lxc exec test-container3 -- su mylogin` to get account access to a container,
    
15. `lxc copy test-container3 test-container3-clone` to copy a container,
    
16. `lxc image list images: | grep -i centos` to list prebuilt images,
    
17. `lxc network show lxdbr0` to display information about network interface(s),
    
18. `lxc profile show default` to check the default profile using lxc command,
    
19. `lxc snapshot test-container3 test-container3_snap` followed by `lxc info test-container3` to take snapshot of an instance,
    
20. `lxc restore test-container3 test-container3_snap` to restore an instance from a snapshot,
    
21. `lxc export test-container3 /root/backup/lxd/test-container3_bkp--$(date +'%m-%d-%Y').tar.xz --optimized-storage` to take backup of an instance,
    
22. `lxc import /root/backup/lxd/test-container3_bkp--05-07-2022.tar.xz` followed by `lxc list` to restore instance from a backup, and
    
23. `lxc --help` to check all the options that are available to an LXC command.
    

> Attribution:
> 
> [https://www.cyberithub.com/20-best-lxc-command-examples-to-manage-linux-containers/](https://www.cyberithub.com/20-best-lxc-command-examples-to-manage-linux-containers/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704973890076/16827347-fadb-4e3b-8749-2fe5c9a2a75b.png align="center")

# The Results.

LXD and LXC provide a powerful, flexible, and resource-efficient way to run isolated system instances on a single host. This technology represents the second wave of system isolation technologies, offering certain advantages over traditional virtual machines. The process of installing LXD and using LXC may seem complex at first, but once I understood the basics and familiarized myself with common commands, I could create, manage, and delete containers with ease. Whether I'm setting up a `homelab` or deploying applications in a production environment, mastering LXD/LXC is a valuable skill.

# In Conclusion.

LXD and LXC was a revolutionary system isolation technology. LXD (Linux Daemon) is a container manager that allows me to create and manage containers. LXCs (Linux Containers), on the other hand, are isolated system instances. They ensure that anything within the container doesn't affect other containers or the base operating system. This means multiple container instances can run concurrently on a single host.

Containers are considered the Second Wave of System Isolation Technologies. The First Wave were virtual machines and the Third Wave are WASM/WASI binaries. Each Wave brought their own strengths and weaknesses.

LXD and LXC provide a powerful, flexible, and resource-efficient way to run isolated system instances on a single host. This technology offers certain advantages over traditional virtual machines, making it a valuable skill to master.

Once I understood the basics, I could create, manage, and delete containers with ease. Whether setting up a `homelab` or deploying applications in a production environment, adopting LXD/LXC was a game-changer.

So, have you used LXD and LXC in your projects? What's your experience been like? Share your thoughts in the comments below!

Until next time: Be safe, be kind, be awesome.

> NOTE: All images generated by [ComfyUI](https://github.com/comfyanonymous/ComfyUI) using the [dreamshaper\_8](https://huggingface.co/digiplay/DreamShaper_8/tree/main) checkpoint.