---
title: Docker swarm setup
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 4
---

{{<hero>}}
We're just about ready to start installing apps using Co-op Cloud. Co-op Cloud apps are a set of **Docker** containers. We'll sometimes work with docker directly, so let's go over the concepts involved.
{{</hero>}}

There's some background info below, but if you're already familiar with Docker and Docker Swarm, you can skip ahead to [Installing Docker](#installing-docker).

{{<aside>}}

## Why we use Docker for our apps

Docker is a program for building, deploying and managing **Containers**, which are lightweight, portable packages for an application and all it's dependencies. These were popularised by Docker, and so are sometimes called **Docker Containers**, but there's actually an open standard for how they work as part of the **Open Container Initiative (OCI)**. Here's a [great article](https://medium.com/@fred.j/docker-in-depth-first-part-history-499682db0c12) that describes what containers are in more detail, and how they relate to docker.

### Containers

If we're going to serve a web app on our new **LoRes Node**, in this case a server providing a local copy of Wikipedia, we need a way of packaging that app up. We could install it directly on our server, which would work, but as we add more and more apps, they will end up having conflicting requirements, and it'll be hard to keep their data and security needs separate.

The most common solution to this problem is to use a container for each program you want to run.

### Container orchestration

So, a container can run any software we need on our server, in isolation from other software, and with all the system dependencies. Is that all we need? Are we done here?

Each container is designed to run one **Service**. That could be the back-end for a web app, or maybe a database or email server. If we think of an **App** as being _something that delivers useful functionality to users_, then that might need a few different services. Perhaps a web back-end, AND a database, AND an email server.

So an app is then a collection of services that work together. We know we can run services as containers using a widely used technology like [Docker](https://www.docker.com/), but how to we coordinate and run the collection of services that we need for an app?

In industry, this problem is called "_Container Orchestration_". There are a range of tools designed to help with this.

Open Source developers often use [Docker Compose](https://docs.docker.com/compose/) is a tool, essentially a plugin for docker, that allows you to run a several docker containers at once on the same machine, with some shared resources and configuration.

Large tech companies typically use [Kubernetes](https://kubernetes.io/), but because it has so many features to support everything that the industry needs, it can be very complex to learn and use.

### Docker Swarm

Since docker compose and it's config files is well understood by lots of people, we really want something close to that, which brings us to [Docker Swarm](https://docs.docker.com/engine/swarm/). Docker Swarm is another plugin feature for docker, that introduces the idea of multiple computers joining a 'swarm' which can run services either on specific computers, and (potentially) with a certain level of redundancy.

Docker Swarm also handles the idea of a set of services, which it calls a **Stack**. A stack is defined using a docker compose file, and there are some built in commands for viewing current stacks, seeing what services they contain, and starting or stopping them.

We should acknowledge here that while Docker Compose is well understood by many developers and hobbyists, Docker Swarm is not wildly popular. It sits in an unusual space of being overpowered for hobbyists (who don't need its multi-server support) while being too simple for industry (who needs the more complex features of something like Kubernetes.)

Having said that, with LoRes Mesh focused on community/neighbourhood scale web hosting, Docker Swarm is just perfect for us. So our apps are powered by **Stacks** using **Docker Swarm** - that means their main config file is a `compose.yml` file (_that's the format that docker compose uses, more on that later_) that is compatible with docker swarm.

{{</aside>}}

## Installing Docker

{{<aside>}}

### 💡 Prefer Co-op Cloud instructions?

Co-op cloud refers to the people who setup and administer a server (that we call **Node Stewards**), as **Operators**. Installing Docker is covered in the [Server configuration](https://docs.coopcloud.tech/operators/tutorial/#server-configuration) section of the New operators tutorial.

{{</aside>}}

The following instructions are all related to your account on the Raspberry Pi, so start by logging in with SSH (`ssh lores.local`).

### Install docker packages

There are a few different ways to install Docker on Ubuntu. We're going to use the convenience script here, the same as in the co-op cloud instructions (above). This is obviously a security trade-off, we're trusting that docker doesn't sneak other commands into this publicly viewable but somewhat complex script. However, we're going to run code from Docker with root privileges anyway. If you'd prefer to use the more manual approach instead, follow [these instructions](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository).

So, ssh into the Pi (`ssh lores.local`) and then run:

```bash
curl https://get.docker.com | bash
```

### Using docker without sudo

With that done, let's ensure the that your user on the PI can run docker commands without needing to use `sudo`. We'll do this by adding your user to the _docker_ group. You can see which groups you're a member of currently by running `groups`.

To add yourself to the docker group, run:

```bash
sudo usermod -aG docker $USER
```

For this to take effect, you'll need to log out then in again. Type `exit` to log out, then `ssh lores.node` to log back in.

To prove out docker setup is working, let's run a hello-world container.

```bash
docker run hello-world
```

You should get a nice message saying everything is working.

### Starting a docker swarm

We want to start a docker swarm. A swarm can be more than one computer, and so we could add others to it later. At the moment though, we only need a swarm containing our one Raspberry Pi. To start a swarm, run:

```bash
docker swarm init
```

You'll be given a join token for other computers to join, but you can ignore that for now, it can be retrieved again if needed.

### Creating the Co-op Cloud docker network

You'll need to go ahead and create a docker network to support Co-op cloud apps. Run:

```bash
docker network create -d overlay proxy
```

Unless there's an error, you're good to go.
