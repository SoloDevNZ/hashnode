---
title: "Installing MariaDB and MySQL Workbench."
seoTitle: "MariaDB Installation & MySQL Workbench Setup"
seoDescription: "Install MariaDB in a container, set up MySQL Workbench on homelab, and connect together for efficient database management."
datePublished: Mon May 15 2023 03:00:42 GMT+0000 (Coordinated Universal Time)
cuid: clho9coq004haspnv9yrx2upe
slug: installing-mariadb-and-mysql-workbench
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684097758213/546805e8-9ce2-419d-8717-54e77c40a648.png
tags: ssh, mariadb, rdbms, ops, mysql-workbench

---

## An Introduction.

The purpose of my `homelab` is to have [a place where I can conduct my software experiments](https://solodev.app/my-first-homelab-a-brief-introduction).

The [purpose of installing `LXD`](https://solodev.app/lxd-on-my-homelab) on my `homelab` is so I can use containers to:

* Isolate these experiments,
    
* Protect the underlying OS from contamination, and
    
* Easily delete these experiments once I have collected my results.
    

The operations below show how I:

* Deploy a MariaDB server in a container,
    
* Install MySQL Workbench on my `homelab`, and
    
* Connect MySQL Workbench to the MariaDB service running in a container.
    

There's also a chunk about setting up, and using SSH, the OpenSSH server, and RSA.

## A Simple Ops Deployment.

This operation is simple to understand, easy to execute, and results in a deployed relational database I can access with a GUI, that *isn't* phpMyAdmin, across my LAN.

## **Setting Up a Container for the MariaDB Server.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684115919133/9ac63b05-c400-4f59-bb76-263491aefa7f.png align="center")

On the `homelab` system, I create a new container called "mariadb":

```plaintext
lxc launch ubuntu:22.04 mariadb
```

Then I bash into the container:

```plaintext
lxc exec mariadb bash
```

I update and upgrade the container:

```plaintext
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove -y
```

## Creating a New User (for the Container).

On the `homelab` system, in the container, I create a new user:

```plaintext
adduser brian
```

I add the new user to the 'sudo' group:

```plaintext
usermod -aG sudo brian
```

I exit the container:

```plaintext
exit
```

I log in to the container with the 'brian' account:

```plaintext
lxc exec mariadb -- su brian
```

## Installing the MariaDB Service.

On the `homelab` system, in the container, I install MariaDB:

```plaintext
sudo apt install mariadb-server -y
```

I verify that the service is running:

```plaintext
sudo systemctl status mariadb
```

If needed, I start the MariaDB server:

```plaintext
sudo systemctl start mariadb
```

I run the security script and answer a few questions:

```plaintext
sudo mysql_secure_installation
```

Securing the MariaDB service is important, however, securing the environment in which these services run is also top of the list. Remember: "Black Hats" are trying to penetrate my defences, and I'm trying to push 18 Startups out the door. Security is vital, it's important, it's *ESSENTIAL*. (It's not the 1970s anymore.)

### What is SSH?

Secure Shell, SSH, is a network protocol used to provide secure encrypted communication between two untrusted hosts over an insecure network. It was designed to replace Telnet and other insecure remote shell protocols.

SSH creates a secure connection between a client and a server, allowing for secure file transfers and remote command-line access to the server's operating system or applications. SSH provides confidentiality and integrity, preventing the interception of data, and it also provides authentication, allowing users to establish their identity in the remote system.

SSH is commonly used to securely access servers remotely, to tunnel traffic between machines, to securely transfer files using SFTP, SCP or Rsync, and to execute remote commands securely. The protocol is implemented in a number of tools such as OpenSSH, PuTTY, and WinSCP, and is widely used for remote administration of servers and network devices.

### What is OpenSSH?

OpenSSH is a free and open-source implementation of the SSH protocol. It was developed as a part of the OpenBSD project and is now used on various Unix-like operating systems.

OpenSSH provides several tools, including ssh (secure shell), scp (secure copy), and sftp (secure file transfer protocol), which enable secure remote access and file transfer. It is widely used for remote administration of servers and network devices, and also for secure communication between computers over an insecure network like the Internet.

OpenSSH supports strong encryption algorithms like AES, Blowfish, and 3DES for secure communication, and provides several features like X11 forwarding, TCP/IP tunnelling, and key authentication for improved security and flexibility.

OpenSSH is free to use and distribute under a BSD-style license, and its source code is available for review and modification. It is one of the most widely-used implementations of the SSH protocol and is considered to be the de facto standard for secure network communication.

### What is RSA?

RSA is a public-key cryptographic algorithm used to encrypt and sign data. It was named after its inventors - Ron Rivest, Adi Shamir, and Leonard Adleman in 1977. RSA is widely used in various applications such as secure email, digital signatures, and online banking.

The RSA algorithm involves the use of two keys - a public key and a private key - to encrypt and decrypt data. The public key is used for encryption and is widely shared among users, while the private key is kept secret and is used for decryption.

Here's an example of how RSA encryption works:

1. The sender of the message obtains the public key of the recipient.
    
2. The sender uses the recipient's public key to encrypt the message.
    
3. The encrypted message is sent to the recipient.
    
4. The recipient uses their private key to decrypt the message.
    

RSA is known to be secure, but its security depends on the length of the key used. As computers have become faster, longer keys are required to maintain the same level of security. RSA cypher suite has been widely deployed and trusted, it is included in many SSL/TLS implementations for secure communication.

## Installing OpenSSH in the Container.

By default, OpenSSH is included in the containers I create, but installing OpenSSH is very easy:

```plaintext
sudo apt install openssh-server -y
```

I can check the status of OpenSSH:

```plaintext
sudo systemctl status sshd
```

If needed, I can enable OpenSSH:

```plaintext
sudo systemctl enable --now ssh
```

## Configuring the SSH File in the Container.

On the `homelab`, in the container, I open the "sshd\_config" file:

```plaintext
nano /etc/ssh/sshd_config
```

I edit and save these "sshd\_config" settings:

```plaintext
AllowUsers brian
PermitRootLogin yes
PasswordAuthentication yes
```

> NOTE: I return to this file later in the post specifically to "harden" this container.

I test the SSH configuration:

```plaintext
sshd -t
```

I restart the SSH system:

```plaintext
sudo systemctl restart ssh.service
```

And finally, I exit the container:

```plaintext
exit
```

## Creating, and Using, RSA Keys.

These steps will enable SSH sessions to the container, across the LAN, without needing a password.

### 1/4 - Creating an RSA Key Pair on the Workstation.

On the `workstation` system, I start the ssh-agent:

```plaintext
eval "$(ssh-agent -s)"
```

I open a terminal (`CTRL` + `ALT` + `T`) and generate an RSA key pair called "/home/brian/.ssh/mariadb":

```plaintext
ssh-keygen -b 4096
```

I add my SSH private key to the ssh-agent:

```plaintext
ssh-add /home/brian/.ssh/mariadb
```

### 2/4 - Uploading a Public Key to the Remote Container.

On the `workstation` system, I use "ssh-copy-id" to upload the locally-generated public key to the remote container:

```plaintext
ssh-copy-id -i /home/brian/.ssh/mariadb.pub brian@192.168.188.?
```

> NOTE: I replace the "?" with the actual octet for the container.

### 3/4 - Logging In to the Remote Container.

On the `workstation` system, I login to the “brian” account of the remote container:

```plaintext
ssh -p 22 brian@192.168.188.?
```

> NOTE: I'd change the -p(ort) flag from 22 to whatever value I set in the "sshd\_config" file above.

### 4/4 - Disabling Password Authentication.

On the `workstation` system, I open the "sshd\_config" file in the remote container:

```plaintext
sudo nano /etc/ssh/sshd_config
```

I edit, and save, these "sshd\_config" settings:

```plaintext
PermitRootLogin no
PasswordAuthentication no
```

> NOTE: Other changes I typically include are enabling "Protocol 2" and switching out the default port number of 22 for something less obvious.

I restart the "ssh" service:

```plaintext
sudo systemctl restart ssh.service
```

## Installing MySQL Workbench on My Workstation

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684114555123/64feb628-5949-403c-ad70-627f4543e662.png align="center")

On the `workstation` system, I open a terminal by pressing `CTRL+ALT+T` on my keyboard.

I update and upgrade my `workstation` system:

```plaintext
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove -y
```

I install the Snap package manager:

```plaintext
sudo apt install snapd
```

I install MySQL Workbench:

```plaintext
sudo snap install mysql-workbench-community
```

The Snap package manager will download and install MySQL Workbench.

Once the installation is complete, I can launch MySQL Workbench by searching for it in the applications menu, or by running the following command in the terminal:

```plaintext
mysql-workbench
```

Once MySQL Workbench has launched, I can start using it to manage my MariaDB (and/or MySQL) databases and tables.

## Connecting MySQL Workbench to MariaDB

To connect MySQL Workbench to a MariaDB server running on my LAN, I need to follow these steps:

1. I make sure that the MariaDB server is running on my LAN and that I know its IP address and port number.
    
2. I open MySQL Workbench and click on the + icon next to MySQL Connections, located on the Home tab of the application.
    
3. In the “Set up a New Connection” window, I enter the following details:
    

* Connection Name: A name for my new connection, for example, "MariaDB".
    
* Hostname: The IP address or hostname of my MariaDB server.
    
* Port: The port number that the MariaDB server is listening to. The default port number for MariaDB is 3306.
    
* Username: The username that I use to connect to the MariaDB server.
    
* Password: The password for my MariaDB user.
    

1. I click on "Test Connection" to check if the information I entered is correct. If the connection is successful, I will see a message saying "Successfully made the MySQL connection".
    
2. Once the connection is tested successfully, I click "OK" and will be redirected to the MySQL Workbench dashboard where I can start working with my databases.
    

## In Conclusion.

This post provided instructions on how I use `LXD` to contain a MariaDB server installation, install MySQL Workbench on my `homelab`, and connect MySQL Workbench to the MariaDB service. There was also a bit about the SSH protocol, the OpenSSH server, and generating RSA Key Pairs.

I'll keep pushing out these posts on the details of my operations processes and development processes. I'll also try to keep them small, digestible, and relevant to how I operate.

Until next time: Be safe, be kind, be awesome.