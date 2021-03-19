
<p align="center"><a href="https://crazy-max.github.io/swarm-cronjob/" target="_blank"><img height="128" src="./docs/swarm-cronjob.png"></a></p

<p>
  <h3 align="center">Swarm Cronjob</h3>
</p>

## Table of Contents

- [Table of Contents](#table-of-contents)
- [System Requetiments](#system-requirements)
- [Technology](#technology)
- [Getting Started](#getting-started)
  - [Docker](#docker)
    - [Install docker](#install-docker)
    - [Install docker-compose](#install-docker-compose)
  - [Swarm](#swarm)
    - [Init swarm](#init-swarm)
    - [Join Nodes](#join-nodes)
  - [Clone the repository](#clone-the-repository)
  - [Swarm Cron](#file-structure)
    - [Volume](#volume)
    - [Labels](#labels)
    - [Test](#test)
- [Useful Links](#useful-links)

## Introduction

Create jobs on a time-based schedule on Docker Swarm

## System Requirements

Before starting, it is good to configure some things so that the swarm can work, this step is very important for the services to work perfectly. I'am using ubuntu 18.04 as the operating system for all the nodes in my cluster, so that the configuration is easier, I recommend using the same.

## Technology

These are the technologies that I am using to run all services:

- **[Git](https://git-scm.com/)** version control system.
- **[Ubuntu](https://ubuntu.com/)** This is the operating system I'm using, you can use another one.
- **[Docker](https://docs.docker.com)** to create our containers and services.
  - **[Swarm](https://docs.docker.com/engine/swarm/)** an orchestrator for our cluster that is native to the docker.
- **[Swarm Cronjob](https://opendistro.github.io/)** is responsible for create jobs on a time-based schedule on Docker Swarm

## Getting Started

the following steps will be necessary for you to run the project

## Docker

Docker is a set of platform-as-a-service products that use operating system-level virtualization to deliver software in packages called containers. it is important for us to run our services because the docker good thing about the docker is that it already comes with an orchestrator on it.

### Install docker

installing the docker is very simple, we just need to make a curl on the docker website that refers to downloads and we will have the most current version of it

```bash
curl -fsSL https://get.docker.com | sh
```

if everything went well the docker will already be installed on your machine

now let's run a command to verify that the docker has been properly installed

```bash
docker version
```

You should expect it to return

```bash
Client: Docker Engine - Community
 Cloud integration: 1.0.7
 Version:           20.10.2
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        2291f61
 Built:             Mon Dec 28 16:12:42 2020
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.2
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8891c58
  Built:            Mon Dec 28 16:15:28 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.3
  GitCommit:        269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc:
  Version:          1.0.0-rc92
  GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

### Install docker-compose

Docker composer doesn't install with docker by default you must install it via curl

#### 1. Download the current stable release of Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

If you have problems installing with ```curl```, see **[Alternative Install Options](https://docs.docker.com/compose/install/#alternative-install-options)** tab above.

#### 2. Apply executable permissions to the binary:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

---
**Note:** If the command ```docker-compose``` fails after installation, check your path. You can also create a symbolic link to ```/usr/bin``` or any other directory in your path.

---

For example:

```bash 
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

1. Optionally, install **[command completion](https://docs.docker.com/compose/completion/)** for the bash and zsh shell.

2. Test the installation.

```bash 
docker-compose --version
docker-compose version 1.28.0, build 1110ad01
```

### Swarm

Cluster management integrated with Docker Engine: Use the Docker Engine CLI to create a swarm of Docker Engines where you can deploy application services.

#### Init Swarm

For you to run this service you will need to have 3 nodes running, only 1 needs to be a manager. this command init swarm in your machine, run this command on the machine you want as the primary

```bash
docker swarm init --advertise-addr <IP_ADRESS>
```

#### Join Nodes

After starting swarm the docker will return you a command, that you will run on te other machines to add them to the cluster.

If you want to add other machines as a manager, run this command and then on the machine you want to give this privilege.

```bash
docker swarm join-token manager
```

```bash
docker swarm join --token SWMTKN-1-50441psf9k11bd0i9b9p17spxhwtog5fvy4v52zvvl11h5g82y-4vknh73kdw1z0cdufhiqmxc04 192.168.0.23:2377
```

Run this command below to be able to check your nodes

```bash
docker node ls
```

You should expect it to return

```response
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
nnc23sjm0nhbrc2wyznt4rz3l *   node1               Ready               Active              Leader              19.03.6
fcrpjwwjw60cpr487sc01dqr7     node2               Ready               Active                                  19.03.6
m8sudulzg66adg0qawl63206t     node3               Ready               Active                                  19.03.6
```

After your 3 nodes are running, we will have to add some labels to them

### Clone the repository

Clone the repository for your main machine.

```bash
git clone https://github.com/Kledenai/swarm-cronjob.git
```
