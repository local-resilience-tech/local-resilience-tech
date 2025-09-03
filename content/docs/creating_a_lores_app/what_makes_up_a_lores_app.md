+++
title = 'What makes up a LoRes App?'
date = 2025-09-03T09:00:00+10:00
draft = false
+++

Before we build our own app, let's take a look inside a LoRes App and see what they're made up of.

We know that the goal of an app is to run open source software on a small server (or group of servers in one place) that we call a **Node**. This is really just a type of [self-hosting web apps](<https://en.wikipedia.org/wiki/Self-hosting_(web_services)>), with a few bells and whistles that allow the apps on a LoRes node to work well together, and in some cases to work with other nodes in the region that make up a **Mesh**.

## Application Services as Containers

Given that, we want to stick with common ways of self-hosting that are already well supported and documented. The most common of these is to use **Containers**, which are lightweight, portable packages for an application and all it's dependencies. These were popularised by a product called **Docker**, and so are somtimes called **Docker Containers**, but there's actually an open standard for how they work as part of the **Open Container Initiative (OCI)**. Here's a [great article](https://medium.com/@fred.j/docker-in-depth-first-part-history-499682db0c12) that descibes what containers are in more detail, and how they relate to docker.

So, a container can run any software we need on our server, in isolation from other softare, and with all the system dependencies. Is that all we need? Are we done here?

Each container is designed to run one **Service**. That could be the backend for a web app, or maybe a database or email server. If we think of an **App** as being _something that delivers useful functionality to users_, then that might need a few different services. Perhaps a web backend, a database AND an email server.

So an app then is a collection of services that work together. We know we can run services as containers using a widely used technology like [docker](https://www.docker.com/), but how to we coordinate and run the collection of services that we need for an app?

In industry, this problem is called "_Container Orchestration_".

## Container Orchestration

So obviously we aren't the first people to have this problem. In fact, it isn't just hobbiests self-hosting their favourite open source web apps that use containers, but most big tech products use them too. Those large scale products have some pretty serious needs, they might have multiple containers spread across different data-centers, even different continents, and it all needs to work together in a secure and highly available way.

### Option 1: Kubernetes

Now we did say that we'd try and use the most common and well understood technologies for LoRes Apps, which means we have to look here at the most common industry approach to container orchestration, [Kubernetes](https://kubernetes.io/) (or "_K8s_" if you want to sound all skibidi or something).

Kubernetes may be the most common container orchestration tool, but because it has so many features to support everything that the industry needs, it can be very complex to learn and use. We were hesitant to choose it here because, frankly, we're a little scared of it.

Just before we move on, we should acknowledge that there's a tool called [K3s](https://k3s.io/) which is a simpler, lighter-weight form of Kubernetes that may be a reasonable contender here.

### Option 2: Docker Compose

So let's go to the other end of the spectrum, what's the simpliest tool for this that's commonly used?

[Docker Compose](https://docs.docker.com/compose/) is a tool, essentially a plugin for docker, that allows you to run a several docker containers at once on the same machine, with some shared resources and configuration. It has a standard config file, that specifies how each of the containers should be configured, and then a single command `docker compose up` for starting the whole group of services.

Overall, it's much closer to the complexity that we need, with the only real downsides being that:

- It doesn't support spreading services across more than one machine
- There's no clear system for sharing or versioning docker compose files (also true for Kubernetes, we'll come back to this later)

### Bridging the Gap with Docker Swarm

Since docker compose and it's config files is well understood by lots of people, we really want something close to that, which brings us to [[Docker Swarm]](https://docs.docker.com/engine/swarm/). Docker Swarm is another plugin feature for docker, that introduces the idea of multiple computers joining a swarm of compute that can run services either on specific computers, or with a certain level of redundancy.

Docker Swarm also handles the idea of a set of services, which it calls a **Stack**. A stack is defined using a docker compose file, and there are some built in commands for viewing current stacks, seeing what services they contain, and starting or stopping them.

We should acknowledge here that while Docker Compose is well understood by many developers and hobbyests, Docker Swarm is not heavily used. It sits in an unusual space where most hobbiests don't need multiple-server support, and industry needs the more complex features of Kubernetes.

Having said that, LoRes Mesh if focussed on community/neighbourhood scale web hosting, and this is just perfect for us. So, our our apps are powered by **Stacks** using **Docker Swarm**. That means their main config file is a `compose.yml` file (_that's the format that docker compose uses, more on that later_) that is compatible with docker swarm.

## Is an App just a compose file?

Well no, not quite. There's a few other key things we need from an app that aren't supported by container orchestration systems.

### App descovery, versioning and upgrades

For each container there's an **Image** which defines how it's going to run. And, there are **Container Image Repositories** which are libraries of all the available images and all their versions. This means that to install an image, and start a container, you can usually just specify what you want and where to fetch it from. An example of this is [Docker Hub](https://hub.docker.com/).

These repositories are great, but there seems to be nothing like it for stacks. Reluctantly, we had to build our own for LoRes. We wanted it to be really simple, and we didn't want people to have to run some custom "_LoRes App Respository_" service. So, we've built a system where any **Git** repository can be considered to be a repository of one or more LoRes Apps, and we look there for the app configuration.

That means, we need a way to specify a few bits of information about the App that don't go in the compose file, including the current version of the app. So, when we talk about building a LoRes App, we'll go over our config file format, and also how we use git tags to specify the available versoins in a repository.

All this requires knowledge of [git](https://git-scm.com/), which is a little complex when you first learn it, but is used in basically all software development (and is pretty handy for a bunch of other things).

### Template variables

A LoRes Node needs some bits of the compose file to be setup in specific ways, like perhaps to allow the node to dictate the base URL of an app. We do this with template variables. Docker compose files already have [a system for that](https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/) using environment variables. So we use that. When we go through how to setup your compose file, we'll cover all the variables that a LoRes Node provides.

### And more...?

There may be more coming here. We're not quite sure. As we build our first few real-world apps, we're going to hit missing things.

For example, an app may want to allow the node to specify some configuration options. Perhaps we want a config file format for that, or perhaps we want to allow the app to specify it's config needs in a way that lets us generate an admin user interface in the Lores Node web interface.

As you're working through building your first app, have a think about what features you need to make it easy for **Node Stewards** to install, administer, maintain and monitor. We'd love to hear what ideas you come up with.
