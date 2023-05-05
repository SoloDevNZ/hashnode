---
title: "My First Homelab: A Brief Introduction."
datePublished: Tue Apr 18 2023 10:00:17 GMT+0000 (Coordinated Universal Time)
cuid: clgm3g9mn000309l4bckhgy6n
slug: my-first-homelab-a-brief-introduction
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682597998515/d5e3c96c-e717-4c63-b97e-18a6b0b553cf.png
tags: startup, homelab

---

---

## Requirements.

This lab uses the following equipment:

* The homelab NUC\* system that runs Ubuntu 22.04 LTS.
    

> NOTE: NUC\* stands for Next Unit of Computing which is nothing more than a compact, small form factor computer.

## An Introduction to this Series.

![intro](https://images.pexels.com/photos/6980429/pexels-photo-6980429.jpeg?cs=srgb&dl=pexels-ann-h-6980429.jpg&fm=jpg&w=640&h=427 align="left")

My name is Brian and I'm a solo developer. Being a solo dev has its' advantages (I get to try everything, *especially* the interesting bits) but there are many disadvantages, too (like having to learn everything, *including* the boring bits).

Building a homelab is new to me. That is why I want to share the processes that I find useful and relevant to setting up a homelab.

## Using Multiple Computers.

![two computers](https://images.pexels.com/photos/115655/pexels-photo-115655.jpeg?cs=srgb&dl=pexels-lee-campbell-115655.jpg&fm=jpg&w=640&h=427 align="left")

From Windows operating systems to Linux distributions, I'm very comfortable with using multiple types of computers. I'm also happy to use remote services like [Linode](https://www.linode.com/), [Digital Ocean](https://www.digitalocean.com/), and [Heroku](https://www.heroku.com/). Testing and hosting my programs within my *own* LAN, on my own local area network, is a novel idea to me. Actually, I'm quite good at systems operations, so building a homelab *should* be easy. However, I've never *actually* built one before, so we'll have to see how things work out.

I’m going to use two separate computers: A `workstation` and a `homelab`. I could have used a single computer and got the same results, but I'm using two machines because there's *also* a networking component that needs investigating.

| Description | Workstation PC | Homelab NUC |
| --- | --- | --- |
| CPU | AMD Ryzen 5 2600 | Intel Core i3-10110U |
| RAM | 16GB (2x8GB) | 40GB (1x8GB, 1x32GB) |
| Main Storage | 1TB M.2 SSD | 256GB M.2 SSD |
| Base Clock | 3.4GHz | 2.1GHz |
| Boost Clock | 3.9GHz | 4.1GHz |
| Cores | 6 | 2 |
| Threads | 12 | 4 |

There are many use cases for building a homelab. Creating one may be difficult, but the value it *might* bring to my development pipeline *could* be worth the effort.

## Using Linux Distributions.

![terminal](https://images.pexels.com/photos/207580/pexels-photo-207580.jpeg?cs=srgb&dl=pexels-pixabay-207580.jpg&fm=jpg&w=640&h=480 align="left")

I use Linux distributions on both the `homelab` system and the `workstation` system. I started using CentOS around 2015 but then I switched to Ubuntu sometime in 2018. My reason for switching to Linux was simple: Publishing to the web is easier if I know how to use the OS that "is used to power 96.3% of the world's top web servers."

This now begs the question: What's my main reason for deploying a homelab?

## The Purpose of my Homelab.

![homelab](https://images.pexels.com/photos/442150/pexels-photo-442150.jpeg?cs=srgb&dl=pexels-field-engineer-442150.jpg&fm=jpg&w=640&h=427 align="left")

I've been interested in programming since the late 1980s when I first used Microsoft BASIC. Obviously, things have changed since then.

It's taken A. VERY. LONG. TIME. to get to a point where modern tools can help me build the stuff that's in my head.

Machine learning, deep learning, and artificial intelligence are beginning to show promising results. As of this writing, everyone is losing their minds over ChatGPT 4. The real magic, however, is the algorithms that build these models.

One reason for this series of blog posts is to show how I’d build a prototype mechanism for hosting machine-learning models. Building these models requires more resources than the `workstation` or `homelab` can provide. But designing a system for building machine learning models is a great topic... For another day. For now, I just want to host these models.

Did you notice that, in the last paragraph, I used the word "prototype"? I'm a fan of experimental systems but there are costs with running these kinds of exercises. I'm constantly repeating processes with slight variations to find closer approximations to a solution. I'm always defining a problem, or a solution, in terms of a simpler version of itself. I'm constantly changing my equipment. I'll replace the dotenv file that controls environment constants. I'll alter the processing order based on the results that I generate. I'd even revise my assumptions if that'll get me closer to an ideal outcome. All of this tinkering generally results in two outcomes: Lots of lab results, and a somewhat unstable system.

I rely on two tools that help with restoring my systems to their stable conditions.

The first tool is Clonezilla Live. I use it to easily image, i.e. clone, individual machines. Clonezilla Live only works when the PC is offline because it needs to boot from a CD/DVD or USB flash drive. That's why I only use it when I've made, or am about to make, significant software changes to an operating system. Clonezilla Live is beyond the scope of these blogs.

The other tool is LXD, pronounced: "lexdee". Note: The next post in this series will describe how I install, and use, LXD to my advantage.

The question is: What's the point? Why go to all the trouble of setting up a homelab?

The simple answer is: I want to build my own tech startup. I want to use machine learning models. I want to create my own apps. And I want to use these apps to generate an income.

## DigitalCore: The Core Business.

![laptop](https://images.pexels.com/photos/2312369/pexels-photo-2312369.jpeg?cs=srgb&dl=pexels-andrew-neel-2312369.jpg&fm=jpg&w=640&h=427 align="left")

The key function of every company is to provide products and services to its customers and clients. Digital Core (NZ) Limited is my tech startup and is NOT in a position to generate a profit... because it's NOT closing sales... because it does NOT have any products, or services, to sell.

As a solo developer, I need to turn my technical designs and product designs into actual products.

As Employee Number One at my startup, I also need to assume the responsibilities of a manager and company director.

Just to clarify: I’m the *only* employee at my startup so I'm the *only* one who is doing *any of the work*.

Being a systems analyst, system designer, product designer, programmer, business manager, bookkeeper, salesperson, and company director imposes HUGE constraints on achieving my professional goals in a timely manner. My goals include making money, spending money, and making *more* money. As a result, I need tools that help with:

* Working precisely,
    
* Working quickly, and
    
* Working economically.
    

Having said that, I need to understand the difference between the function of my company and its core business. After all, I don't want to build any tools that achieve the wrong outcomes. Take this list, for example:

| Company | Key Function | Core Business |
| --- | --- | --- |
| McDonald's (fast-food chain) | Selling burgers and fries | Real estate |
| Facebook | Social platform | Advertising |
| Google | Search engine | Advertising |
| DigitalCore (my startup) | Lifestyle utilities | Advertising |

All of my products include specialist features that help differentiate them from the competition, yet include the same bog-standard features as their rivals. The *ACTUAL* purpose of my products is to *satisfy my core business objective*: To sell advertisements. It's disappointing that my core business is the same as everyone else. However, it's a core business objective that is easy to understand... And it works when applied correctly.

To achieve my core business objective, I need a stable, adaptable, and repeatable system on which to build, test, and deliver my products. I also need this platform to promote SECURITY as *the PRIMARY consideration*. I need toolchains that ensure development pipelines and delivery platforms place security at *the forefront of every publishing cycle*.

## Toolchains, Pipelines, and Security.

![security](https://images.pexels.com/photos/60504/security-protection-anti-virus-software-60504.jpeg?cs=srgb&dl=pexels-pixabay-60504.jpg&fm=jpg&w=640&h=427 align="left")

The tools that I use, the order in which I use them, and how these tools help to build secure mechanisms, will go a long way to ensuring positive user experiences (even though my security practices will be transparent to the user) while *also* complying with international policies regarding personal data collection, retention, and erasure.

Therefore, the first *technical* step to achieving this outcome is to harden (or *at least* start the process of hardening) my `homelab` system.

## Series Direction.

![compass](https://images.pexels.com/photos/3832684/pexels-photo-3832684.jpeg?cs=srgb&dl=pexels-joshua-woroniecki-3832684.jpg&fm=jpg&w=640&h=409 align="left")

I want to take this moment to clarify the direction for this series. I'll try to cover the technical aspects of setting up, and using, a homelab. I'll also recount the business practices that I consider essential. I want to include all the steps I take when completing a specific task, but cannot respond to requests for help due to my lack of resources and experience. (I'd like to help but, at this time, I can barely help myself.)

## In Conclusion.

![conclusion](https://images.pexels.com/photos/6980525/pexels-photo-6980525.jpeg?cs=srgb&dl=pexels-ann-h-6980525.jpg&fm=jpg&w=640&h=427 align="left")

This post covered:

* The technologies I use,
    
* The *main* purpose of my Homelab,
    
* The function and objective of my startup,
    
* The reasons for hardening a system, and
    
* The direction this series will take.
    

I hope you found this brief useful. The rest of the posts in this series will be very hands-on and comes with plenty of examples and commands.

Be sure to check out the next post in this series where I use LXD on my homelab.