---
# try also 'default' to start simple
theme: default
presenter: true
title: 'Cloud Computing | Virtualization: VMs and Containers'
titleTemplate: '%s - CPIT-490'
# apply any windi css classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: | 
    Cloud Computing | Virtualization: VMs and Containers
# page transition
transition: slide-left
# use UnoCSS
css: unocss
# Make content selectable/copyable
selectable: true
favicon: '/images/favicon.ico'
# Make slides downloadable as PDF
download: true
exportFilename: virtualization-slides
export:
  format: pdf
  timeout: 30000
  dark: false
  withClicks: false
  withToc: false
# enable slide recording and drawing
record: build
drawings:
  enabled: true
  persist: false
  presenterOnly: false
  syncAll: true
hideInToc: true
---

# Virtualization: VMs and Containers

### CPIT-490/632: Cloud Computing


<div class="absolute left-30px bottom-30px">
Spring 2024 &copy; Khalid Alharbi, Ph.D.
</div>


---
hideInToc: true
---

# Table of Contents

<Toc columns="2" maxDepth="1" mode="all" class="toc-list"/>

---
layout: fact
---


# Virtualization

## Virtualization enables running multiple operating system instances concurrently on the same physical machine.



---

## Virtualization

- Virtualization software manages computing resources (CPU, memory, and I/O) and their use among **guest** operating systems.
- Virtualization increases efficiency because the hardware is utilized to handle multiple guests simultaneously.
- Guest operating systems are isolated from each other and access the hardware through the hypervisor.

---

# Virtualization and Hypervisors
- A **Hypervisor** is a software layer that sits between the virtual machines (VMs) and the underlying hardware to handle sharing resources among guest operating systems.
- For example, you can have Linux (CentOS, Ubuntu, etc.), Unix (FreeBSD), and Windows running alongside.
- There are two types of hypervisors: Type 1 and Type 2

---

# Type 1 and Type 2 Hypervisors
<br>

![](/images/hypervisor-typ1-and-2.png)


---

# Type 1 Hypervisor (Bare Metal)

- Runs directly on the host's hardware to control the hardware and to manage guest operating systems.
- Offers better performance and efficiency because it has direct access to hardware.
- Generally used in cloud provider environments where performance, visibility, scalability, and reliability are critical.
- Examples include VMware ESXi, Microsoft Hyper-V, and Xen.

---

# Type 2 Hypervisor (Hosted)

- Runs on a host operating system just like other applications on the system.
- Performance is considered lower because it has to go through the host operating system to access hardware resources.
- Generally used in desktop environment for testing or personal use.
- Examples include Oracle VirtualBox, VMware Workstation, and Parallels Desktop.


---

## Machine Image
A machine image is a single static unit that contains a pre-configured operating system and installed software packages which are used to create new running machines.


---


# The Works on my Machine Problem (I)
- One common problem in software development is "works on my machine" phenomenon.
- Many developers set up a development environment with tools and libraries.
- The environment grows up over time with more added libraries and more changes to configuration options.
- The local development environment becomes very different from the target production environment.
- Libraries that are present on the development system may not exist on the production system.
- This leads into an unexpected errors and runtime behaviors.

---

## Solutions to the Works on my Machine Problem
- Create an isolated, dedicated development environment locally for each project that matches the production environment.
  - **Option 1**: Provision a custom VM with all the required development environment locally using virtualization software.
  - **Option 2**: Develop in a more managed, smaller, and isolated environment such as a _Container_ environment.



---


# Infrastructure as Code Approach
- All the previous solutions are central to the **infrastructure as code** approach.
- Infrastructure as code is the practice of managing and provisioning OS environments through machine-readable definition files, rather than manually applying changes to VM images and installing dependency libraries and tools.


---

# Creating and Managing Custom VM Images
- There're many virtualization systems that facilitate creating and managing virtual machine images.
  -  vSphere, the VMware virtualization suite of products.
- There're special tools for building and managing portable virtual machine images and environments.
  - [Vagrant](https://www.vagrantup.com/)
  - [Packer](https://packer.io)
  

---

# Vagrant
- Vagrant is an open source tool for provisioning and managing virtual machine environments.
- Vagrant can manage development environments running on a local virtualized platform such as VirtualBox or VMware, in the cloud via AWS, Azure, or any other provider, or in containers such as Docker.
- Vagrant can be used to create and manage an Infrastructure as a Service (IaaS)-like environment locally.


---

## Vagrant: Getting Started (I)
1. Install a hypervisor such as Oracle VirtualBox or VMWare Fusion.
2. [Install Vagrant](https://www.vagrantup.com) on your local environment.
3. Install a plugin for your hypervisor:
  - Vagrant comes with support out of the box for VirtualBox. However, if you are using VMware, you will need to install a plugin `vagrant plugin install vagrant-vmware-desktop`
4. You will need to pick a Vagrant Box, the package format for Vagrant environments.
5. Boxes can be installed locally from an image or from the [publicly available catalog of Vagrant boxes](https://vagrantcloud.com/boxes/search). 


---

## Vagrant: Getting Started (II)
- You will need to use the following commands:
  - `vagrant init <name_of_vm_image>`
  - `vagrant up`
  - `vagrant ssh`

---
layout: center
---

##  Using Vagrant (I) `vagrant init`

<br>

```bash
vagrant init debian/bookworm64
```

- The command above will create a file called _Vagrantfile_ that contains all the provisioning options to customize your VM (e.g., the amount of memory on the VM).
- `debian/bookworm64` is the name of the box, which is [Debian 12 (Bookworm)](https://app.vagrantup.com/debian/boxes/bookworm64).
- A box is a package format for Vagrant environments


---
layout: center
---

##  Using Vagrant (II) `vagrant up`

<br>

```bash
vagrant up
```

- The command above will download, start, and provision the vagrant VM environment.


---
layout: center
---

##  Using Vagrant (III) `vagrant ssh`

<br>

```bash
vagrant ssh
```

- The command above will connect to the local VM from the host OS via SSH
- To shut down the VM instance, use the command `vagrant halt`


---
layout: center
---

## Vagrant Demo


---

# Packer (I)

<br>

- [Packer](https://packer.io/) is an open source tool for building virtual machine images for multiple platforms from a single configuration file.
- Packer can be used to generate new machine images for multiple platforms with the changes made to them.
- This is in particular helpful in keeping development and production environments identical.
- To create an image, Packer launches a VM instance from a source image of your choice.
- Packer can customize the VM instance by running commands and scripts that may install libraries or customize configuration files.
- Finally, packer saves a copy of the Virtual Machine's state as an image.

---

## Packer (II)

<br>

- Packer and Vagrant are both tools developed by HashiCorp and they serve different but complementary purposes.
  - Vagrant creates and manages development environments abstracting away the need for direct interaction with the graphical user interface of the hypervisor (e.g., VirtualBox or VMware).
  - Packer Packer creates identical machine images for multiple platforms from a single configuration file.
- Packer uses a template configuration file in JSON format.
- Packer's template file defines how to create an image, add provisioning options, and install software and libraries for the image.
- To build the custom image, run the command `packer build <custom_image.json>`


---


## Packer: Getting Started

<br>

1. Download the Packer binary file from [https://packer.io](https://packer.io/)
2. Create a packer template file
3. Run 

```bash
packer build
```

---
layout: center
---

## Packer: Demo


---
layout: center
---

## Wrapping Up: Vagrant and Packer

- You can use Packer to create machine images with your application and all of its dependencies, and then use Vagrant to manage environments where these images run locally. 

- Both Vagrant and Packer are powerful tools to create identical and reproducible development, testing, and production environments.


---

# Docker Containers


- Docker is a platform for developing, deploying, and running applications in Linux containers. Docker is a popular tool for many reasons:

  - __Containerization__: Operating system level virtualization.
  - __Lightweight__: Containers are often smaller in size than virtual machines and share the host kernel.
  - __Portable__: You can develop your app locally and deploy it on the cloud and run it anywhere where you can run Docker.
  - __Manageable__: You can break a large monolithic application into a collection of small units, where each  unit runs in its own container. This technique is known as _Microservices_.
  - __Scalable__: You can increase the number of containers and create identical replicas of them.


---
layout: two-cols-header
---

# Docker vs Virtual Machines

::left::

## Docker

- Containers are OS level virtualization
- Containers are lightweight as they use shared operating systems
- Containers are much more efficient than hypervisors in terms of system resource usage.
- Startup time is faster, usually in milliseconds, as they run on the host OS.
- Containers are less isolated than VMs. They share the OS kernel, libraries, and even their binary running protocol.
- Docker containers are ideal for microservice architecture because they provide process isolation.


::right::

## Virtual Machines

- Virtual Machines (VMs) are hardware-level virtualization.
- VMs are large with their own full system tools and resources.
- Startup time is slower, usually in minutes, as they need to boot up their own OS.
- VMs are completely isolated from the host system and other VMs.
- VMs are ideal for running applications that require all of the operating system's resources and functionality.

---

## Download and Install Docker

- Please refer to the instructions on the Docker documentation at: [docs.docker.com/install](https://docs.docker.com/install/) to install Docker Community Edition (CE) on your platform. 
- Docker works in a client-server architecture. 
  - You have a long-running background/daemon process (the `dockerd` command)
  - You also have a docker client (the `docker` command) that communicates with the daemon process by passing requests to it .

---

# Docker CLI
You will use the `docker` command line interface (CLI) tool to work with Docker containers. It's recommended that you check the available commands and options using `docker --help`. To make sure that you have successfully installed and started the docker daemon is running, run:

```bash
docker info
```

This command should return common statistics (e.g., number of containers and images) and basic configuration.

```
Containers: 24
 Running: 0
 Paused: 0
 Stopped: 24
Images: 30
...

```
---

## Images vs Containers
<br>

- A **Docker image** is an executable package that includes everything needed to run an application (OS, libraries, and configuration files). 
  - It is the building block that docker uses to build containers. 
  - A docker image is made up of file systems layered over each other.
  - You can see a list of your Docker images with the command, `docker images`.

- A **Docker container** is the running instance of an image. 
  - When an image is running, it's called a container. 
  - You can see a list of your running containers with the command, `docker ps`.

---

# Dockerfile

<br>

- A _Dockerfile_ is a text document that contains all the commands or instructions to build a docker image.

```
FROM alpine:latest
RUN apk update && apk add nano
CMD ["/bin/sh"]
```

  - The Alpine Linux distribution is a lightweight, secure, and simple Linux distribution commonly used as the base layer in Docker images.
  - `apk` is the package manager for Alpine Linux similar to `apt` in Debian.
  - `RUN apk update && apk add nano` will update system packages and install nano.
  - `CMD ["/bin/sh"]`. This is the default command that the container runs, which is starting a new shell session


---

## Build your first Docker image
To build an image, you need to select a base image from a resource or a repository such as [hub.docker.com](https://hub.docker.com). Then, you may customize the image to your need.

- To build an image from the _Dockerfile_ shown above, run:

```bash
docker build -t hello-docker:v1 .
```
  - This will build a new image from the _Dockerfile_ with the name hello-docker and tag v1.
  - The `-t` option adds a name and a tag to the image in the 'name:tag' format.
  - The `.` argument points to the PATH that contains the _Dockerfile_, which is the present working directory.

- To run the created image, run:

```bash
docker run -i -t hello-docker:v1
```
  - This will run the command defined in the _Dockerfile_, which was running the bash command, in a new docker container.
  - The `-i` option will run in interactive mode to keep the STDIN open
  - The `-t` option will allocate a pseudo-TTY. This attaches your terminal to the interactive shell in the container.
    - These two options are often combined as `-it`

---

## Dockerize a Java application (I)

1. Create a simple Java program with the following content:
   
      App.java
      
      ```java
      public class App {
      
        public static void main(String[]args){
          System.out.println("Hello Java!");
        }
      }
      ```

---


## Dockerize a Java application (II)

2.  In order to compile Java files on a docker container, you need to have a container image with the Java Development Kit (JDK).
    - Search the [docker hub](https://hub.docker.com) for an image with _openjdk_. The search results show various images. We will pick the official `openjdk` image with the tag `8-jdk-alpine` because it's based on Alpine Linux, which is a minimal Docker image based on Alpine Linux with a very small size (around 5 MB).
        
    - The Java program can be compiled using the command `javac App.java`, which returns a compiled file named App.class that can be run using the command `java App`. We will issue these commands inside the container.


---

## Dockerize a Java application (II)

2. Create a file named _Dockerfile_ in the same directory that the above java code is in with the following content:
   
```
from openjdk:8-jdk-alpine
RUN mkdir /app
COPY App.java /app
RUN javac /app/App.java
WORKDIR /app
CMD ["java", "App"]
```

3. Build an image from the _Dockerfile_

```bash
docker build -t java-container:latest .
```
4. Run the container
   
```bash
docker run java-container:latest
```



