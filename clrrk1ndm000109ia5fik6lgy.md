---
title: "Installing NVM for Node and NPM."
datePublished: Wed Jan 24 2024 09:00:25 GMT+0000 (Coordinated Universal Time)
cuid: clrrk1ndm000109ia5fik6lgy
slug: installing-nvm-for-node-and-npm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706172468211/10111675-00d7-4151-b609-47bc6265a83b.png
tags: nodejs, npm, node, nvm, node-js, node-version-manager, node-package-manager

---

# TL;DR.

This post provides a step-by-step guide on how to install NVM (Node Version Manager) for managing Node and NPM versions. It covers everything from the prerequisites and installation of CURL to installing and activating NVM. It then details how to use NVM to install Node LTS and NPM, setting Node LTS as the default NVM version, and confirming the installations. It also includes instructions on how to test the installation by creating and running a simple Node.js project.

> **Attributions:**
> 
> [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)***↗, and***
> 
> [https://www.freecodecamp.org/news/how-to-install-node-js-on-ubuntu/](https://www.freecodecamp.org/news/how-to-install-node-js-on-ubuntu/).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058792137/dc96dd05-e3dd-4b7d-8e02-0010c2afcb57.png align="center")

# An Introduction.

Node is a commonly-used JavaScript runtime that uses the V8 engine from Google. NPM (Node Package Manager) usually ships with Node. NVM (Node Version Manager) is a utility that manages Node versions on my system.

> The purpose of this post is to use NVM to install the latest Node LTS and NPM.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058738171/d0240ae2-066f-4013-83e2-6fd0444d069d.png align="center")

# The Big Picture.

NVM (Node Version Manager) is capable of much more than is described in this post. For instance, I can have multiple versions on Node installed on my system(s) and, thanks to NVM, I can easily switch between any of them.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058764853/3ff5e17b-d55d-4ee8-a309-699670372445.png align="center")

# Prerequisites.

* A Debian-based Linux distro (I use Ubuntu).
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058819339/4c70bd64-9a99-4f94-b5c3-f8c760f341e6.png align="center")

# Installing CURL.

* I update my system:
    

```bash
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt --fix-broken install && sudo apt autoremove -y
```

* I install CURL:
    

```bash
sudo apt install -y curl
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058883515/ad1334b3-b6c9-423c-a80e-70d479219579.png align="center")

# Installing the NVM.

* I install the NVM (Node Version Manager):
    

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

> NOTE: The version numbers are [listed on the GitHub page](https://github.com/nvm-sh/nvm/tags).

* I activate the NVM:
    

```bash
source ~/.bashrc
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058896044/3fcb9698-ca79-482d-8201-ecb1b604d403.png align="center")

# Using NVM to Install Node LTS and NPM.

* I use NVM to install the LTS version of Node:
    

```bash
nvm install --lts
```

> NOTE: The Node and NPM versions are listed on the screen.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705058995257/3a0ae3eb-08df-43a4-92a3-58f1cda178d8.png align="center")

# Making Node LTS the Default NVM Version.

* I make Node LTS the default NVM version, as listed in the previous section:
    

```bash
nvm alias default 20.11.0
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705059161714/e4a4cd7e-3261-42b2-9e8d-36486234342a.png align="center")

# Confirming the Installations.

* I confirm the Node installation:
    

```bash
node -v
```

* I confirm the NPM installation:
    

```bash
npm -v
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705059789177/760b9d75-ac62-4f92-a97b-3f04002a5651.png align="center")

# Testing the Installation.

* I make, and change into, a new directory:
    

```bash
mkdir ./test-node-project && cd ./test-node-project
```

* I initialize a new Node project:
    

```bash
npm init -y
```

> NOTE: This command will create a `package.json` file:
> 
> ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705007002426/8354a297-a7b2-4952-8508-f2c049580d27.png align="center")

* I use the Nano text editor to create a file called `app.js`:
    

```bash
sudo nano app.js
```

* I add the following, save (CTRL + S) the changes, and close (CTRL + X) the Nano text editor:
    

```plaintext
console.log("Hello, Node.js from Ubuntu!");
```

* I run the following command:
    

```bash
node app.js
```

> NOTE: A successful test result has the "Hello, Node.js from Ubuntu!" message printed to the terminal.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705059281951/2aafc759-bd9b-4c65-94cb-25017f0bc51d.png align="center")

# The Results.

Installing NVM on a Debian-based Linux system like Ubuntu allows for easy management of Node.js and NPM versions. This guide walked through the necessary steps, from installing CURL and NVM, to installing and setting the default Node LTS and NPM versions. The successful execution of a simple Node.js project confirmed the installation processes. With NVM, I can now effortlessly switch between different Node versions as needed, enhancing my development workflow.

# In Conclusion.

I've struggled with managing different versions of Node.js and NPM on my system, but here's the solution: Node Version Manager (NVM). NVM lets me install, manage, and switch between different versions of Node.js and NPM with ease.

I've crafted a step-by-step guide on how I install and use NVM on a Debian-based Linux system (like Ubuntu). From installing the prerequisites like CURL, to setting up NVM, and finally running a simple Node.js project to test the installation, this guide has me covered.

Key takeaway? NVM let's me effectively control my Node.js environment, there by enhancing my development workflow. Node.js version management is now easier and more efficient.

Have you tried NVM? How was your experience? Share your thoughts in the comments!

Check out [the original](https://www.freecodecamp.org/news/how-to-install-node-js-on-ubuntu/)[freecodecamp.org](http://freecodecamp.org)[article](https://www.freecodecamp.org/news/how-to-install-node-js-on-ubuntu/).

Until next time: Be safe, be kind, be awesome.

> NOTE: All images generated by [ComfyUI](https://github.com/comfyanonymous/ComfyUI) using the [juggernautXL\_version6Rundiffusion](https://huggingface.co/frankjoshua/juggernautXL_version6Rundiffusion/tree/main) checkpoint.