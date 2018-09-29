---
title:  "What Is Docker?"
tags: [devops, docker]
---

[Docker](https://www.docker.com) is the probably the *hottest* technology right now in the technology space. In this post, i am writing about the basic of docker and the challenges it solves.

## What Is Docker?

- Docker is a platform which can be used to **package, distribute and run** your applications.

- Docker provides an easy and efficient way to encapsulate applications (e.g. a Java web application) and any required infrastructure to run that application (e.g. Red hat Linux OS, Apache web server, Tomcat application server, mySQL database etc.) as a single **“Docker image”**.

- The image can then be used to launch a **Docker container** which makes the **containerized** application available from the host.

## DevOps Matrix From Hell

![cname file content]({{ site.url }}/assets/img/matrix-from-hell.png)

**DevOps Matrix From Hell** is the challenge of packaging any application, regardless of language/frameworks/dependencies, so that it can run on any cloud, regardless of operating systems/hardware/infrastructure. This is due to the multipicity of many language/technology/framework stacks that need to installed on so different hardware and operating system environments.

![cname file content]({{ site.url }}/assets/img/container-for-code.png)

Docker act as a system container for code. It encapsulate all the dependencies within a container. The container can then be run the same way in any environment. Docker solved for the matrix from hell by decoupling the application from the underlying operating system and hardware. It did this by packaging all dependencies inside Docker containers, including the OS. 


![cname file content]({{ site.url }}/assets/img/matrix-from-hell-2.png)

This makes Docker containers "portable", i.e. they can run on any cloud or machine without the dreaded "it works on this machine" problems. The manipulation and operations of the docker containers is the same for all supported hardware platforms.



## Containers vs Virtual Machines

![cname file content]({{ site.url }}/assets/img/virtual-machines.png)

Docker container is a kind of light weight virtual machine with considerably smaller memory and disk space footprint than a full blown virtual machine.

Virtual machines take up a lot of system resources. Each VM runs not just a full copy of an operating system, but a virtual copy of all the hardware that the operating system needs to run. This quickly adds up to a lot of RAM and CPU cycles. In contrast, all that a container requires is enough of an operating system, supporting programs and libraries, and system resources to run a specific program.

What this means in practice is you can put two to three times as many as applications on a single server with containers than you can with a VM.


## Seperation Of Concerns

![cname file content]({{ site.url }}/assets/img/seperation-of-concerns.png)

- Developers can focus on packaging applications and dependencies as containers, without worrying about underlying infrastructure. 
- Developers worries about what is *inside* the container. e.g. code, libraries, package manager, apps, data, etc

- Operators can focus on managing containers, without worrying about the contents of those containers.
- Operators worries about what is *outside* of the container. e.g. networking, logging, monitoring, dynamic configuration.


## For Developers

- Build once... (finally) run anywhere.
- No worries about missing dependencies, packages, or mismatched dependencies versioning.
- Reduce/eliminate concerns about compatibility on different platforms, either your own or your customers.
- Consistent development environments for your entire team. All developers use the same OS, same system libraries, same language runtime, no matter what host OS they are using
- Deployment is easy. If it runs in your container, it will run on your server just the same.

## For Operators

- Deployment is more efficient, consistent, and repeatable.
- Eliminate inconsistencies between development, test, production, and customer environments.
- Support segregation of duties.
- Significantly improves the speed and reliability of continuous deployment and continuous.
- containers are so lightweight, address significant performance, costs, deployment, and portability issues normally associated with virtual machines.

## Deep Dive

Docker works by using linux containers and kernel facilities.

![cname file content]({{ site.url }}/assets/img/docker-internals.png)

- Linux kernel namespaces provides the isolation.
- Linux kernel cgroups provide the resource limiting ( cpu, memory, I/O )
- AUFS provide the union file system for docker. ( to avoid duplicating a complete set of files each time you run an image as a new container)


