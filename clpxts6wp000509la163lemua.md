---
title: "Installing Ollama."
datePublished: Sat Dec 09 2023 09:00:12 GMT+0000 (Coordinated Universal Time)
cuid: clpxts6wp000509la163lemua
slug: installing-ollama
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703975053907/4415f003-b120-40f4-becd-28cb06b1fb6a.png
tags: ai, artificial-intelligence, machine-learning, deep-learning, llm, large-language-models, local-llm

---

Published: Saturday 12<sup>th</sup> December 2023.  
Update: Sunday 31<sup>st</sup> December 2023 (yes, I'm working on New Year's Eve.)  
Update: Tuesday 20<sup>th</sup> February 2024.  
Update: Sunday 10<sup>th</sup> March 2024.

# TL;DR.

This post describes the installation of Ollama, a local large language model (LLM) manager. It requires a Linux-based distro and Miniconda. After setting up a Miniconda environment, I install Ollama, which is used to download and run LLMs. I use the Mistral model as an example. Ollama enables the use of powerful LLMs for research, development, business, and personal use.

> ***Attributions:***
> 
> [Ollama.ai](https://ollama.ai/)↗.

# An Introduction.

LLMs (large language models) are amazing machines. They are the bleeding edge of modern technology and devs should learn, and adapt to, these awesome engines.

> The purpose of this post is to describe how to install Ollama, a local LLM manager.

# The Big Picture.

There are many open-source LLMs. Ollama provides easy access to a small set of those models. Visit the [Ollama models library](https://ollama.ai/library) to get a list of the LLMs they support. The index is continually updated, so I frequently revisit this archive.

I love Hugging Face, but it's *also* nice to have a curated series of models.

# Prerequisites.

* A Debian-based Linux distro (I use Ubuntu),
    
* [Miniconda](https://solodev.app/installing-miniconda).
    

# Updating my Base System.

* In the base terminal, I update my base system:
    

```python
sudo apt clean && \
sudo apt update && \
sudo apt dist-upgrade -y && \
sudo apt --fix-broken install && \
sudo apt autoclean && \
sudo apt autoremove -y
```

# What is Anaconda and Miniconda?

Python projects can run in virtual environments. These isolated spaces are used to manage project dependencies. Different versions of the same package can run in different environments while avoiding version conflicts.

venv is a built-in Python 3.3+ module that runs virtual environments. Anaconda is a Python and R distribution for scientific computing that includes the `conda` package manager. Miniconda is a small, free, bootstrap version of Anaconda that also includes the `conda` package manager, Python, and other packages that are required or useful (like pip and zlib).

[http://www.anaconda.com/](http://www.anaconda.com/)***↗,***

[https://docs.anaconda.com/free/miniconda/index.html](https://docs.anaconda.com/free/miniconda/index.html)***↗, and***

[https://solodev.app/installing-miniconda](https://solodev.app/installing-miniconda).

I ensure [Miniconda is installed](https://solodev.app/installing-miniconda) (`conda -V`) before continuing with this post.

## Creating a Miniconda Environment.

* I use the `conda` command to display a `list` of Miniconda `env`ironments:
    

```bash
conda env list
```

* I use `conda` to create, and activate, a new environment named (-n) (Ollama):
    

```bash
conda create -n Ollama python=3.11 -y && conda activate Ollama
```

> NOTE: This command creates the (Ollama) environment, then activates the (Ollama) environment.

## Changing the `Ollama` Home Directory.

> NOTE: I will define the home directory with settings in the environment directory.

* I create the Ollama home directory:
    

```bash
mkdir ~/Ollama
```

* I make new directories within the (Ollama) environment:
    

```bash
mkdir -p ~/miniconda3/envs/Ollama/etc/conda/activate.d
```

* I use the Nano text editor to create the `set_working_directory.sh` shell script:
    

```bash
sudo nano ~/miniconda3/envs/Ollama/etc/conda/activate.d/set_working_directory.sh
```

* I copy the following, add it (CTRL + SHIFT + V) to the script, save (CTRL + S) the changes, and exit (CTRL + X) Nano:
    

```bash
cd ~/Ollama
```

* I activate the (base) environment:
    

```bash
conda activate
```

* I activate the (Ollama) environment:
    

```bash
conda activate Ollama
```

> NOTE: I should now, by default, be in the `~/Ollama` home directory.

# What is `Ollama`?

`Ollama` is a tool that is used to download, set up, and run large language models on a local PC. It lets me use powerful models like Llama 2 and Mistral on my personal computer. `Ollama` natively runs on Linux, macOS, and Windows.

## Installing Ollama.

* I install Ollama:
    

```bash
curl https://ollama.ai/install.sh | sh
```

* I list the LLMs downloaded by Ollama:
    

```bash
ollama list
```

* If the above command fails, I run Ollama as a background service:
    

```bash
ollama serve &
```

* If the following error shows when running the previous command, that means Ollama is *already* running as a background service:
    

```bash
Error: listen tcp 127.0.0.1:11434: bind: address already in use
```

## Using Ollama to Run the Mistral Model.

* I run the Mistral model:
    

```python
ollama run mistral
```

> NOTE 1: The `ollama run` command performs an `ollama pull` if the model has not already been downloaded.

> NOTE 2: The `ollama run` command is used to run the named LLM.

> NOTE 3: I sometimes need to restart my system to properly run the LLM.

## Testing the Mistral Model.

* At the `>>>` prompt, I run the following command:
    

```python
tell me a joke
```

> NOTE: Do NOT expect the joke to be any good.

# The Results.

By following the steps outlined in this post, I can set up dedicated Anaconda environments, install Ollama, download LLMs from [Ollama.ai](https://ollama.ai/), and test them locally. This process enables me to evaluate the power of cutting-edge models for research, development, business, and personal use.

> NOTE: Commercial use depends on the licenses of each model.

# In Conclusion.

LLMs are the bleeding edge of modern technology and we should adapt to these awesome engines.

I've been exploring Ollama, a local LLM manager that provides easy access to a selection of open-source LLMs from [Ollama.ai](https://ollama.ai). Their list of supported LLMs is continually updated. It's a buffet of cutting-edge tech that keeps on growing!

The installation process was straightforward because all I needed was a Linux-based distro and [Anaconda](https://solodev.app/installing-anaconda). I created and activated a new environment named (Ollama) using the `conda` command. Then, I set up an Ollama home directory and made new directories within the (Ollama) environment. Once I got my environment set up, I installed Ollama and started downloading and running an LLM. I've been testing it out with the 'Mistral' model. And let me tell you, it's impressive!

By following these steps, I can set up dedicated Anaconda environments, install Ollama, download LLMs from [Ollama.ai](http://Ollama.ai), and test them locally.

This is a game-changer for research, development, business, and personal use. Future Tech is here and it's time to embrace it.

Have you used Ollama? What has been your experience? Let's discuss in the comments below!

Until next time: Be safe, be kind, be awesome.