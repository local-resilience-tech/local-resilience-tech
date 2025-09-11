---
title: Your LoRes App development environment
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 3
type: docs
---

{{< hero >}}
If you're going to work on LoRes Apps, you're going to need a setup on your own computer that makes it easy to try them out.
{{< /hero >}}

That means temporarily running the **LoRes Node** software on your computer. You can of course test out a LoRes App on a server running a LoRes Node, but that's going to make it hard to tweak and test things as you go.

## Prerequisites

What sort of computer and software will you need to be able to do this?

- **Linux**. You'll need to be running linux on your computer. It's probable that other unix-like systems would work. For the rest of these docs, we're going to assume you're running Ubuntu or Debian linux.
- **Git**. You'll need git installed, and somewhere you can share your repository with others. We recommend [Codeberg](https://codeberg.org/).

Other things that you'll need we'll cover how to install.

{{< aside >}}

### Do I really need to be running linux?

So you probably already know that Linux is an open source operating system, an alternative to Windows or Mac OSX. Running it on your computer, either as your only OS, or by dual-booting so you can use it occasionally, is a great idea because it's free, open, and really nice to use.

The reason for mentioning it here though isn't to get your to change your computing life, but because Linux is the most common choice for web servers, and it's what we need to run on our LoRes Nodes. If you run it on your own machine, it just it easier to follow these instructions.

Want help getting started with Linux? There's probably a Linux club or user group where you live that would live to help you out. For example, [here's a list](https://linux.org.au/lugs/) for Australia.

More advanced users can probabaly translate these instructions for Mac OSX, or for one of the several ways of running linux inside Windows.
{{< /aside >}}

## Step 1. Install Docker

First up you'll need to install Docker. Official docker install instructions are found [here](https://docs.docker.com/engine/install/). You can select the linux distribution you have and get a detailed guide.

Note that official docker instructions recommend that you uninstall any docker packages that come with your linux distro, and add an official package repository provided by docker. This method works fine, but also not that the packages provided by linux distros like Ubuntu or Debian actually also work fine. So on Ubuntu, it's actually ok to just run `sudo apt install docker.io`.

## Step 2. Start a Docker Swarm

So we mentioned in ["what makes up a lores app"](../what_makes_up_a_lores_app) that Docker Swarm is how we orchestrate service containers. To get setup for that, you need to initialise a **Swarm**.

A docker swarm collects multiple computers together to run run replicate **Services** and **Stacks** (groups of services) across them. Each computer in a swarm is referred to as "node" by docker swarm, but let's refer to that here as a **Swarm Node** to avoid confusion with a **LoRes Node** which is actually the entire swarm (but is a node, or point of connection, from the point of view of the LoRes Mesh spread out across our neighbourhood).

For developing apps, we don't really need a proper swarm of multiple computers, we just need to start a swarm with our development computer as the one-and-only node in a swarm. To do that, run:

```
sudo docker swarm init
```

You should see some output like:

```
Swarm initialized: current node (XXXXXX) is now a manger.

To add a worker to this swarm, run the following command:

    docker swarm join --token SOMETOKENHERE

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

If this was a real swarm, it'd be important to save the token that it provides (where I wrote `SOMETOKENHERE`) somewhere safe, because you'd need that to add more computers to the swarm. We're just using this for development and we can delete the swarm whenever we're done, so you can basically just ignore that.

Note that when you've done that, your computer is now running docker swarm, and will continue to do so even if you reset it. No real harm in that, but you can remove that swarm at any point in the future by running:

```
sudo docker swarm leave --force
```

If you're also already using docker swarm for any other purpose on your computer, that should be find for developing LoRes Apps. We just need a swarm running, we don't really mind why it's running.

If you'd like to test whether your swarm is running, you can run `sudo docker stack ls` to list the current stacks (there might be none), and if there's no current swarm you'll get a helpful error message.

## Step 3. Run the LoRes Node Stack

Now we get to the fun part. To build an app, you're going to want **LoRes Node** running. We aren't writing code on LoRes Node itself, so we don't have to worry about building it ourselves. Instead, we can just install and run it from a docker registry.

If you're interested, LoRes Node is currently hosted in the github container repository, you can see details about it [here](https://github.com/local-resilience-tech/lores-node/pkgs/container/lores-node).

As well as LoRes Node, we also need a tool called [Traefik](https://doc.traefik.io/traefik/), which is an application proxy that does routing of urls to different parts of the stack (including both the LoRes Node software, and the App that you're going to build).

You don't need to install that yourself though, we've put together a docker compose file that will configure a stack that contains LoRes Node, Traefik and a bit of shared config. You need to grab that file and save it locally on your hard disk somewhere.

There are two ways to do that. You can use [this direct link to the file](https://raw.githubusercontent.com/local-resilience-tech/lores-node/refs/heads/main/docker-swarm-app-dev.yml) and use your browser to save it locally. Or, if you're comfortable with git, you can clone [the lores-node git repository](https://github.com/local-resilience-tech/lores-node) and reference the file from there (noting that it doesn't depend on anything else in the repository). The only advantage of the git approach is that you can pull more recent versions if we change the file.

Once you've got that file you can deploy the stack using this command.

```
sudo docker stack deploy -d -c ./docker-swarm-app-dev.yml lores-node
```

(Replace `./docker-swarm-app-dev.yml` with the path to the file if that isn't right)

Try it out, you should have the LoRes node software running, and you can access it at:

[http://localhost:8200](http://localhost:8200)
