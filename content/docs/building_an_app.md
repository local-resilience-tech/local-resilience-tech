+++
title = 'Building a LoRes App'
date = 2025-09-03T09:00:00+10:00
draft = false
type = 'docs'
+++

## What makes up a LoRes App?

Before we build our own app, let's take a look inside a LoRes App and see what they're made up of.

We know that the goal of an app is to run open source software on a small server (or group of servers in one place) that we call a **Node**. This is really just a type of [self-hosting web apps](<https://en.wikipedia.org/wiki/Self-hosting_(web_services)>), with a few bells and whistles that allow the apps on a LoRes node to work well together, and in some cases to work with other nodes in the region that make up a **Mesh**.

### Application Services as Containers

Given that, we want to stick with common ways of self-hosting that are already well supported and documented. The most common of these is to use **Containers**, which are lightweight, portable packages for an application and all it's dependencies. These were popularised by a product called **Docker**, and so are somtimes called **Docker Containers**, but there's actually an open standard for how they work as part of the **Open Container Initiative (OCI)**. Here's a [great article](https://medium.com/@fred.j/docker-in-depth-first-part-history-499682db0c12) that descibes what containers are in more detail, and how they relate to docker.

So, a container can run any software we need on our server, in isolation from other softare, and with all the system dependencies. Is that all we need? Are we done here?

Each container is designed to run one **Service**. That could be the backend for a web app, or maybe a database or email server. If we think of an **App** as being _something that delivers useful functionality to users_, then that might need a few different services. Perhaps a web backend, a database AND an email server.

So an app then is a collection of services that work together. We know we can run services as containers using a widely used technology like [docker](https://www.docker.com/), but how to we coordinate and run the collection of services that we need for an app?

In industry, this problem is called "_Container Orchestration_".

### Container Orchestration

So obviously we aren't the first people to have this problem. In fact, it isn't just hobbiests self-hosting their favourite open source web apps that use containers, but most big tech products use them too. Those large scale products have some pretty serious needs, they might have multiple containers spread across different data-centers, even different continents, and it all needs to work together in a secure and highly available way.

#### Option 1: Kubernetes

Now we did say that we'd try and use the most common and well understood technologies for LoRes Apps, which means we have to look here at the most common industry approach to container orchestration, [Kubernetes](https://kubernetes.io/) (or "_K8s_" if you want to sound all skibidi or something).

Kubernetes may be the most common container orchestration tool, but because it has so many features to support everything that the industry needs, it can be very complex to learn and use. We were hesitant to choose it here because, frankly, we're a little scared of it.

Just before we move on, we should acknowledge that there's a tool called [K3s](https://k3s.io/) which is a simpler, lighter-weight form of Kubernetes that may be a reasonable contender here.

#### Option 2: Docker Compose

So let's go to the other end of the spectrum, what's the simpliest tool for this that's commonly used?

[Docker Compose](https://docs.docker.com/compose/) is a tool, essentially a plugin for docker, that allows you to run a several docker containers at once on the same machine, with some shared resources and configuration. It has a standard config file, that specifies how each of the containers should be configured, and then a single command `docker compose up` for starting the whole group of services.

Overall, it's much closer to the complexity that we need, with the only real downsides being that:

- It doesn't support spreading services across more than one machine
- There's no clear system for sharing or versioning docker compose files (also true for Kubernetes, we'll come back to this later)
