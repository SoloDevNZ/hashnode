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

### **Setting Up a Container for the MariaDB Server.**

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

### Creating a New User (for the Container).

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

### Installing the MariaDB Service.

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

I test the SSH configuration:

```plaintext
# sshd -t
```

I restart the SSH system:

```plaintext
sudo systemctl restart ssh.service
```

And finally, I exit the container:

```plaintext
exit
```

### Creating, and Using, RSA Keys.

These steps will enable SSH sessions to the container, across the LAN, without needing a password.

### 1/4 - Creating an RSA Key Pair on the Workstation.

On the `workstation` system, I open a terminal (`CTRL` + `ALT` + `T`) and generate an RSA key pair called "/home/brian/.ssh/mariadb":

```plaintext
ssh-keygen -b 4096
```

After generating the SSH keys, I start the ssh-agent:

```plaintext
eval "$(ssh-agent -s)"
```

Once the ssh-agent is running, I add my SSH private key to the ssh-agent:

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

> NOTE: I'd change the -p(ort) flag from 22 to whatever value I set in the "sshd\_config" file.

### 4/4 - Disabling Password Authentication.

On the `workstation` system, I open the "sshd\_config" file in the remote container:

```plaintext
sudo nano /etc/ssh/sshd_config
```

I edit and save these "sshd\_config" settings:

```plaintext
PermitRootLogin no
PasswordAuthentication no
```

> NOTE: Other changes I typically include are changing to "Protocol 2" and switching out the default port number for something less obvious.

I restart the "ssh" service:

```plaintext
sudo systemctl restart ssh.service
```

### Installing MySQL Workbench on My Workstation

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

### Connecting MySQL Workbench to MariaDB

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