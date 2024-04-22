---
title: "What is Technology?"
datePublished: Mon Mar 04 2024 09:00:31 GMT+0000 (Coordinated Universal Time)
cuid: cltcpnuz2000409kyaq77bf9q
slug: what-is-technology
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1709191615668/1ba33f01-8bf8-45f4-ab61-71991981a755.png
tags: cloudflare, github, technology, python, git, miniconda, streamlit, anaconda, chromadb, ollama, anythingllm, tech-talk, gitlab-ce, digital-world, chroma

---

# TL;DR.

This post provides an overview of various technologies used in the digital world. It covers Python and Git for coding, Docker and LXC for creating virtual environments, AnythingLLM and Ollama for AI capabilities, and Cloudflare and Streamlit for web hosting and data management. It emphasizes the importance of understanding these technologies to excel in the online landscape.

# An Introduction.

Using a template provides a standard layout the helps with parsing the content of my posts. This blueprint continues to evolve as new methods of presenting information are adopted.

> The purpose of this post is to centralise the short descriptions, and attributions, that are used as segment headings in my articles.

# The Big Picture.

As part of my ever-evolving layout, I've decided to centralise a specific part of my template. Below are small descriptions of specific technologies that are used to preface new coding and/or operations segments in my posts.

# What is Anaconda and Miniconda?

Python projects can run in virtual environments. These isolated spaces are used to manage project dependencies. Different versions of the same package can run in different environments specifically to avoid any conflicts between these versions.

venv is a built-in Python 3.3+ module that runs virtual environments. Anaconda is a Python and R distribution for scientific computing that includes the `conda` package manager. Miniconda is a small, free, bootstrap version of Anaconda that also includes the `conda` package manager, Python, and other packages that are required or useful (like pip and zlib).

[http://www.anaconda.com/](http://www.anaconda.com/)***↗,***

[https://docs.anaconda.com/free/miniconda/index.html](https://docs.anaconda.com/free/miniconda/index.html)***↗, and***

[https://solodev.app/installing-miniconda](https://solodev.app/installing-miniconda).

[Miniconda must be installed](https://solodev.app/installing-miniconda) (`conda -V`) before continuing with this post.

# What is AnythingLLM?

AnythingLLM is a free, easy-to-use, and versatile document chatbot. It is made for people who want to chat with, or create, a custom knowledge base using existing documents and websites. RAG (Retrieval-Augmented Generation) is the process of creating custom knowledge bases, which conveniently sidesteps the need for finetuning an LLM (Large Language Model) with LoRA (Low-Rank Adaptation) or QLoRA (Quantised LoRA). The information from the documents and websites are converted, and saved, to a vector database so an LLM can use this data to improve its answers my queries. AnythingLLM can work with many users, or just a single user, from one installation. This makes AnythingLLM great for those who want a ChatGPT experience, and complete privacy, while supporting multiple users in the same setup.

[https://useanything.com/](https://useanything.com/)***↗.***

# What is Chroma?

Chroma is an open-source vector database for saving, and using, embeddings. Its main job is to store embeddings, with related information, for use by large language models. Chroma works well as a foundation for text-based search engines and is perfect for handling lots of unstructured and partly structured data.

[https://www.trychroma.com/](https://www.trychroma.com/)***↗.***

# What is Cloudflare?

Cloudflare is a worldwide network of data centres that offer content and services that make websites and apps faster, safer, and more reliable. It's one of the biggest networks in the world. Companies, non-profits, bloggers, and anyone with an online presence enjoy quicker, more secure websites and apps because of Cloudflare. It supports millions of internet properties, and its network grows by tens of thousands daily. Cloudflare handles internet requests for millions of websites and manages, on average, 55 million HTTP requests per second.

[https://www.cloudflare.com/](https://www.cloudflare.com/)***↗.***

# What is Distrobox?

Distrobox is a tool that allows me to run any Linux distribution within my terminal. It enables both backward and forward compatibility with any software, and the freedom to use whichever distribution I'm most comfortable with. Distrobox uses Podman, Docker, or Lilipod to create containers using the Linux distribution of my choosing. It aims to run any software, on top of my host system, without any hassle.

[https://distrobox.it/](https://distrobox.it/)***↗.***

# What is Docker?

Docker is a tool for easily deploying, and running my applications on any platform. I can package an application with all its code, libraries, dependencies, and tools, which allows me to deploy that app as a single bundle. Docker guarantees that my application will run on any computer that also runs Docker.

[https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers](https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers)***↗,***

[https://solodev.app/installing-docker](https://solodev.app/installing-docker), and

[https://www.docker.com/***↗***](https://www.docker.com/%E2%86%97)[***.***](https://www.docker.com/)

[Docker must be installed](https://solodev.app/installing-docker) (`docker -v`) before continuing with this post.

# What is Docker Compose?

Docker Compose is a tool that helps define and run applications with multiple containers. It makes development and deployment smoother and more efficient. Compose makes it simple to control my whole application stack, managing services, networks, and volumes in one easy-to-understand YAML configuration file. I can create, and start, all the services from my configuration file with just one command. Compose works in all environments, like production, staging, development, testing, and CI/CD. It also has commands to manage my applications' entire lifecycle.

[https://docs.docker.com/compose/](https://docs.docker.com/compose/)***↗.***

# What is a Docker Container?

Docker Containers are ephemeral (temporary) system simulations that run on PCs. Containers, which are created from Docker Images, are virtual spaces that separates my running applications from the base OS and other apps. Containers are easy to run on other systems that also support Docker.

[https://docs.docker.com/guides/walkthroughs/what-is-a-container/](https://docs.docker.com/guides/walkthroughs/what-is-a-container/)***↗.***

# What is Docker Desktop?

Docker Desktop is a developer utility for my Mac, Linux, or Windows environment that lets me build, share, and run containerized applications and microservices. It provides a straightforward GUI (Graphical User Interface) that lets me manage my applications, containers, and images directly.

[https://docs.docker.com/desktop/](https://docs.docker.com/desktop/)***↗.***

# What is Docker Hub?

Docker Hub is a place where developers and open source contributors can find, use, and share container images. It lets developers host public repos for free or private repos for teams and businesses. Docker Hub is the biggest collection of container images, with content from community developers, open source projects, and software companies creating and sharing their code in containers.

[https://docs.docker.com/docker-hub/](https://docs.docker.com/docker-hub/)***↗.***

# What is a Docker Image?

Docker Images are immutable (permanent) files that are used to run code in Docker Containers. They also serve as a guide for building Docker Containers, like a template. Images are the starting point when using Docker. They are similar to a snapshot in virtual machine (VM) environments.

[https://docs.docker.com/build/building/packaging/](https://docs.docker.com/build/building/packaging/)***↗.***

# What is Docker Scout?

Docker Scout helps keep software safe by analysing images, quickly finding vulnerabilities, giving suggestions to fix problems, and more. It is made for developers and works well with Docker. With Docker Scout, I spend less time looking for, and fixing, issues and more time working on my code. The Docker Scout CLI plugin is a powerful, yet flexible terminal interface.

[https://docs.docker.com/scout/](https://docs.docker.com/scout/)***↗.***

# What is Docker Swarm?

Docker Swarm mode is part of the current versions of Docker and helps manage a group of Docker Engines called a swarm. With the Docker CLI, I can create a swarm, put app services into it, and control how the swarm works. Docker Swarm mode is built directly into the Docker Engine.

[https://docs.docker.com/engine/swarm/](https://docs.docker.com/engine/swarm/)***↗.***

# What is Gatsby?

Gatsby is an open-source, React-based, static site generator (SSG) framework. It is used to build websites that are performant, scalable, and secure. I can also pull data from a headless CMS.

[https://www.gatsbyjs.com/](https://www.gatsbyjs.com/)

# What is Git?

Git is a version control manager created by Linus Torvalds in 2005 and, since then, this project has been maintained by Junio Hamano. This utility helps me track the changes I make to my code. Git is used by development team members, who collaborate on software projects, to merge their individual changes to a local, or remote, repository (repo) at the end of each day.

[https://git-scm.com/book/en/v2/Getting-Started-What-is-Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)***↗.***

[Git must be installed](https://solodev.app/installing-git) (`git -v`) before continuing with this post.

# What is GitHub?

GitHub hosts a collection of remote repos where local changes to a code base can be saved to an off-site location. These remote repos can either be public or private. Competing Git-based repos include GitLab and Bitbucket.

[https://docs.github.com/en/get-started/using-git/about-git](https://docs.github.com/en/get-started/using-git/about-git)***↗.***

# What is GitLab CE?

GitLab CE (Community Edition) is an open source, end-to-end software development platform with built-in version control, issue tracking, code review, CI/CD, and more.  
I can self-host GitLab CE on my own servers, in a container, or on a cloud provider.

[https://gitlab.com/free-releases/gitlab-ce](https://gitlab.com/free-releases/gitlab-ce)***↗.***

# What is GPT Pilot?

GPT Pilot is the main engine that powers the Pythagora VS Code extension. (The developers of Pythagora want it to be the first, *true* AI assistant for developers.) Together, GPT Pilot and Pythagora do more than just fill in missing code or help with pull request messages. GPT Pilot helps Pythagora act like a real AI developer that can write complete features, fix bugs, discuss problems, request reviews, and do many other tasks related to coding.

# What is Jekyll?

Jekyll is a free, Ruby-based, open-source tool for creating static websites. I don't need to know Ruby; It just needs to be installed. It is ideal for various static websites, such as personal blogs, portfolios, and documentation sites, eliminating the need for databases or content management systems. Jekyll works with text and markdown files, converting them into secure static HTML files, which can be easily hosted without complex systems. It is secure, does not require a database or server-side scripts, and is highly customizable. Additionally, Jekyll has a large community that provides inspiration and assistance.

[https://jekyllrb.com/docs/](https://jekyllrb.com/docs/)***↗.***

# What is Jupyter Notebook?

Jupyter Notebook is an open source web application that lets me create, and share, documents that include live code, equations, visuals, and text. It is managed by the maintainers at Project Jupyter and started as part of the IPython project. The name "Jupyter" reflects the main languages it supports: Julia, Python, and R. It comes with the IPython kernel for Python programming, but I can also use over 100 other kernels.

[https://jupyter-notebook.readthedocs.io/en/latest/](https://jupyter-notebook.readthedocs.io/en/latest/)***↗.***

# What is LangChain, LangSmith, LangeServe, and LangGraph?

LangChain is a Python and JavaScript library that helps me build language model applications. It uses these models to help with tasks like answering questions, creating text, or performing other tasks.

LangSmith is a tool for creating professional LLM applications. It helps me debug, test, check, and keep an eye on chains, and smart agents, made with any LLM framework, and is designed to work smoothly with LangChain.

LangeServe is a platform for deploying and maintaining my LLM applications. It helps me turn any LangChain runnable, or chain, into a REST API, making my LangChain projects ready for real-world use.

LangGraph is a tool for creating complex applications with LLMs that involve multiple parts working together. It is designed to work with LangChain and adds new features to the LangChain Expression Language, helping me manage several chains (or parts) through many steps in a loop.

[https://python.langchain.com/docs/get\_started/introduction](https://python.langchain.com/docs/get_started/introduction)↗,

[https://docs.smith.langchain.com](https://docs.smith.langchain.com)↗,

[https://www.langchain.com/langserve](https://www.langchain.com/langserve)↗, and

[https://python.langchain.com/docs/langgraph](https://python.langchain.com/docs/langgraph)↗.

These LangChain tools must be installed (`pip show langchain`) before continuing with this post.

# What is LiteLLM?

LiteLLM is a proxy that provides LLMs with IP addresses. It is a unified interface that calls 100+ LLMs using the same Input/Output format, supporting OpenAI, Huggingface, Anthropic, vLLM, Cohere, and even custom LLM API services.

[https://litellm.ai/](https://litellm.ai/)***↗.***

[LiteLLM must be installed](https://solodev.app/installing-litellm) (`litellm -v`) before continuing with this post.

# What is LXD and LXC?

The LXD (LinuX Daemon) is the container manager that is used to create, and manage, LXCs (LinuX Containers). It is a background service that can automatically start LXCs when the host system boots, or stop any container from starting at all.

A LXC (LinuX Container) is an isolated, OS-level virtualization which, for efficiency, uses the Linux kernel of the host system. It is a virtual environment where system processes within the LXC can not affect other containers, or the host system, without specifically running certain commands.

[https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers](https://ubuntu.com/tutorials/how-to-run-docker-inside-lxd-containers)***↗,***

[https://ubuntu.com/server/docs/containers-lxd](https://ubuntu.com/server/docs/containers-lxd)***↗,***

[https://ubuntu.com/server/docs/containers-lxc](https://ubuntu.com/server/docs/containers-lxc)***↗, and***

[https://solodev.app/installing-lxd-and-using-lxcs](https://solodev.app/installing-lxd-and-using-lxcs).

[LXD must be installed](https://solodev.app/installing-lxd-and-using-lxcs) (`lxc --version`) before continuing with this post.

# What is MAX?

MAX (Modular Accelerated Engine) is a set of tools that makes it easier for me to work on AI projects. It includes the MAX Engine, MAX Serving, and the Mojo programming language. These tools help me create, and run, the next generation of machine learning solutions.

[https://www.modular.com/max](https://www.modular.com/max).

# What is MemGPT?

MemGPT is a memory manager that empowers LLMs (large language models) to overcome their limited context windows. It learns about me, modifies its' personality, and when connected to an LLM, assumes the duties of a chatbot. When MemGPT is connected to my own filesystem, tools, databases, and APIs, a new level of LLM proficiency emerges. For instance, combining MemGPT with the AutoGen facility blends pair programming abilities with an infinite memory.

# What are Microservices?

Microservices are small, independent "components" that communicate over well-defined APIs. They are usually developed, and maintained, by small teams of programmers. Each microservice is:

* Small,
    
* Autonomous,
    
* Loosely coupled,
    
* Focused on one task,
    
* Independently deployable,
    
* Aligned with a bounded context, and
    
* An abstraction at the level of the problem domain.
    

The microservice architecture enables an organization to deliver large, complex applications rapidly, frequently, reliably, and sustainably - a necessity for deploying my 12 Startups.

[https://www.ibm.com/topics/microservices](https://www.ibm.com/topics/microservices)***↗.***

# What is NGINX?

NGINX is a free, open-source tool for web hosting, reverse proxying, caching, load balancing, media streaming, and more. It began as a web server focused on top performance and stability. Besides its HTTP server features, NGINX can also work as an email proxy server (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.

[https://www.nginx.com/resources/glossary/nginx/](https://www.nginx.com/resources/glossary/nginx/)***↗.***

# What is NGINX Reverse Proxy?

A reverse proxy is a server that sits behind the firewall in a network and directs client requests to appropriate backend servers. The NGINX Reverse Proxy is a reconfigured NGINX server that provides an additional level of abstraction and control. This go-between server ensures network traffic between clients and servers flows smoothly.

[https://www.nginx.com/resources/glossary/reverse-proxy-server/](https://www.nginx.com/resources/glossary/reverse-proxy-server/)***↗.***

# What is Node.js, NPM, and NVM?

Node.js is a free, JavaScript (JS) server runtime. It runs JS code as single-threaded, non-blocking, asynchronous programs, which makes it very memory efficient. Node.js performs many system-level tasks, but is commonly used to run JS servers.

NPM (Node Package Manager) is the world's largest software registry. Open source developers use it to share packages with each other. Many organizations use NPM to manage private development as well. Most developers interact with NPM using the CLI (Command Line Interface), and usually ships with Node.js.

NVM (Node Version Manager) is used to switch between versions of Node.js. NVM works on any POSIX-compliant shell (sh, dash, ksh, zsh, bash) and runs on Linux distros, macOS, and Windows WSL.

[https://nodejs.org/en/learn/getting-started/introduction-to-nodejs](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)↗,

[https://docs.npmjs.com/about-npm](https://docs.npmjs.com/about-npm)↗,

[https://github.com/nvm-sh/nvm#intro](https://github.com/nvm-sh/nvm#intro)↗, and

[https://solodev.app/installing-node-and-npm-with-nvm](https://solodev.app/installing-node-and-npm-with-nvm).

[Node.js must be installed](https://solodev.app/installing-node-and-npm-with-nvm) (`node -v`) before continuing with this post.

# What is Ollama?

Ollama is a tool for downloading, setting up, and running LLMs (large language models). It lets me access powerful models like Llama 2 and Mistral and helps me run them on my local Linux, macOS, and Windows systems.

[https://ollama.com/↗](https://ollama.com/%E2%86%97).

[Ollama must be installed](https://solodev.app/installing-ollama) (`ollama -v`) before continuing with this post.

# What is Pythagora?

Pythagora is a tool that helps create apps from the ground up using large language models. It's an extension for VS Code and runs on GPT Pilot, one of the best code generators around. While designed around GPT-4, I have adjusted the GPT Pilot settings so Pythagora can work with local LLMs (large language models).

# What is Python?

Python is an easy-to-understand programming language that is ideal for quickly developing applications and integrating various components. It features built-in tools for code organization and reuse. Both the Python interpreter and a comprehensive standard library are freely available across all major platforms, making it a favourite among programmers for its efficiency in speeding up the development process. Unlike compiled languages, Python allows immediate editing, testing, and debugging, which enhances productivity. Debugging in Python is straightforward; instead of crashing, the interpreter presents exceptions, or stack traces, for uncaught errors. It also offers a source-level debugger for inspecting variables, evaluating expressions, setting breakpoints, and step-by-step code walk-throughs, all showcasing Python's versatility. Interestingly, the debugger itself is written in Python. Often, simply adding print statements to the code is the fastest debugging approach, benefiting from Python's quick edit-test-debug cycle.

[https://www.python.org/doc/essays/blurb/](https://www.python.org/doc/essays/blurb/)***↗.***

# What is RSA?

Rivest-Shamir-Adleman (RSA) is a type of encryption that is popular in many products and services. It uses two connected keys to lock and unlock data. There is a private key and a public key. The public key can be shared with anyone, but the private key is kept secret by the person who made the keys. With RSA, the public key can lock the data, but only the private key can unlock it.

[https://en.wikipedia.org/wiki/RSA\_(cryptosystem)](https://en.wikipedia.org/wiki/RSA_(cryptosystem))***↗.***

# What is Rust?

Rust is a systems programming language that empowers everyone to build reliable and efficient software. It is blazingly fast and memory-efficient: with no runtime or garbage collector, it can power performance-critical services and run on embedded devices. Rust is focused on performance, memory safety, and safe concurrency.

[https://www.rust-lang.org/](https://www.rust-lang.org/)***↗.***

# What is Scrapy?

Scrapy is a fast, open-source, extensible, and powerful Python-based scraping framework that is used to extract website data. Although it was originally designed for web scraping, Scrapy can also use APIs to extract data, or even as a general purpose web crawler. Like other frameworks, there is a steep learning curve, however experienced systems operators and software developers should be able to understand the documents, install this utility, and comfortably use Scrapy without too much effort.

[https://scrapy.org/](https://scrapy.org/)***↗.***

# What is SSH?

The Secure Shell (SSH) protocol is a way to safely send commands to a computer over an unsecured network. SSH uses special codes to check and protect connections between devices. It also allows for tunnelling, or port forwarding, which lets data packets go through networks they couldn't before. SSH is often used to control servers from a distance, manage infrastructure, and transfer files. It lets administrators manage servers and devices remotely. Older methods like Telnet sent commands that anyone could see. But SSH uses a secure connection, which is why it's called Secure Shell.

[https://www.cloudflare.com/learning/access-management/what-is-ssh/](https://www.cloudflare.com/learning/access-management/what-is-ssh/)***↗.***

# What is Streamlit?

Streamlit is a free, open-source framework that helps me make web apps from data scripts using Python, even without front-end knowledge. With Streamlit, I can quickly transform my data scripts into shareable apps. After creating an app, I can deploy, manage, share, and interact with that app using the Streamlit Community Cloud.

[https://streamlit.io/](https://streamlit.io/)***↗.***

# What is Twinny?

Twinny is the definitive, no-nonsense, AI code completion plugin for VS Code. It is also compatible with the VSCodium editor. Twinny is designed to seamlessly work with locally-hosted LLMs (large language models), frameworks, and tools. It supports FIM (fill in the middle) code completion using real-time, AI-based suggestions that helps with auto-completion as I type my code. I can also discuss my code via the sidebar, get function explanations, generate tests, request refactoring, and more.

There are many other features that Twinny provides.

[https://marketplace.visualstudio.com/items?itemName=rjmacarthy.twinny](https://marketplace.visualstudio.com/items?itemName=rjmacarthy.twinny↗)↗.

# What is VS Code?

Visual Studio Code (VS Code) is a simple, yet powerful, code editor that works on my computer with versions for Windows, macOS, and Linux. It natively supports JavaScript, TypeScript, and Node.js, and I can add more languages like C++, C#, Java, Python, PHP, Go, .NET, and others through extensions.

[https://code.visualstudio.com/](https://code.visualstudio.com/)***↗.***

# What are Web Page Scrapings?

Web page scraping, or data scraping, is the process of collecting content and data from websites, usually to save it for later use and analysis. Moving information from a website to a spreadsheet is a simple form of web scraping. But when I talk about 'web scrapers,' I'm referring to software 'bots' that automatically visit websites, find important data, and pull out useful information. Almost any data on a website can be scraped and is widely used in data analytics. Web scraping has a dark side and can be used to steal bank details or personal information. It is important to understand the risks, ethics, and legality of web scraping.

[https://beautiful-soup-4.readthedocs.io/en/latest/](https://beautiful-soup-4.readthedocs.io/en/latest/)

# What are Web URL Scrapings?

URLs (uniform resource locations) describe where specific assets (webpages, email services, downloads, etc.) are found. These are usually fed into the address bar of my browser either manually or via a link on a webpage. URL scraping is the process of finding all the URLs at a specified, domain-name address and then saving the results to a file or database.

# The Results.

Technology encompasses an array of tools, platforms, and frameworks that aid in various tasks such as software development, web hosting, data management, and more. From Python and Git for coding, to Docker and LXC for virtual environments, each technology has its unique role and benefits. Tools like AnythingLLM and Ollama leverage AI capabilities to enhance my experience and efficiency. Understanding these technologies and their applications can significantly augment my ability to navigate and excel in today's digital world.

# In Conclusion.

Technology is vast and diverse arena, with each tool, platform, and framework playing a unique role in tasks like software development, web hosting, data management, and more.

In the ever-evolving digital landscape, understanding these technologies can give me a significant edge.

What's your favourite tech tool and why? Let's discuss below!

Until next time: Be safe, be kind, be awesome.

#Technology #TechTalk #Python #Docker #Git #Cloudflare #Streamlit #DigitalWorld #AnythingLLM #Ollama #Anaconda #Chroma #DockerContainers #DockerCompose #DockerDesktop #DockerHub #DockerImages #DockerScout #DockerSwarm #GitHub #GitLabCE #LXC #LXD #Miniconda #NGINX #NodeJS #NPM #NVM #RSA #SSH #WebHosting #DataManagement #SoftwareDevelopment #AI #VirtualEnvironments