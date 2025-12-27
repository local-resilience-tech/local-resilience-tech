---
title: Initial Node Setup
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 1
type: docs
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to_ **setup a node that provides a copy of wikipedia on my local network**
_So that we can_ **access useful information even if the internet goes down**
{{< /user_story >}}

## Our goals

Welcome **Node Steward**. We're going to walk through setting up your first node. Our goal is to keep this fairly simple, so this node will use simple hardware that can be expanded later, and will only have one web application installed, an offline reader for Wikipedia.

Is this useful? We think so. **LoRes Mesh** is all about providing local resilience in the face of the worsening climate crisis. Even if you live somewhere where the internet has always been reliable, storms, heat waves, fires and floods are going to be more and more likely in the coming years and this does mean you could lose internet access or power.

With that in mind, our goal is to get a working low-power computer on your local network serving up a copy of wikipedia, which you can browse to on your local network even if the internet is down.

## Non goals

To keep things quick for this initial setup, we are **not** going to:

- Power your node with a battery, so initially it will only work if the internet is down, not if you've lost power.
- Connect your node to other nodes in your local region, that comes next.

## What you'll need

This is an opinionated tutorial, so while most of these requirements have alternatives, for this documentation we're going to assume you have the following.

- A computer that you'll work from, which we'll call your **Dev Computer**
- A [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/) with case and power supply.
- A MicroSD card, at least 64GB. Empty is best, no need to buy one advertised as specifically for the Pi.
- A MicroSD card reader, sometimes found built into laptops or cheaply available as a USB accessory
- A 256GB USB flash drive ([details](./coop_cloud/flash_drive))
- A local wifi network, presumably with password to access it
- Internet access available on your wifi, at least during this setup phase
- Optional: An ethernet cable, probably a short one

We're also going to assume for the moment that your dev computer is running Linux. In practice this probably isn't strictly necessary, but it makes our instructions a bit simpler.

{{< aside >}}

### Do I really need to be running Linux?

So you probably already know that Linux is an open source operating system, an alternative to Windows or Mac OSX. Running it on your computer, either as your only OS, or by dual-booting so you can use it occasionally, is a great idea because it's free, open, and really nice to use.

The reason for mentioning it here though isn't to get your to change your computing life, but because Linux is the most common choice for web servers, and it's what we need to run on our LoRes Nodes. If you run it on your own machine, it's just easier to follow these instructions.

Want help getting started with Linux? There's probably a Linux club or user group where you live that would live to help you out. For example, [here's a list](https://linux.org.au/lugs/) for Australia.

More advanced users can probably translate these instructions for Mac OSX, or for one of the several ways of running Linux inside Windows.
{{< /aside >}}

## What you're learn

Setting up this node will touch on a range of skills, which are also useful outside of this project. You'll learn about:

- [Git](https://git-scm.com/) - a distributed way of of sharing source code and recording each change to it
- [Ubuntu Linux](https://ubuntu.com/server) - a common and beginner-friendly flavour of Linux
- [Co-op Cloud](https://coopcloud.tech/) - a software stack for hosting web apps built by a federation of tech co-ops
- [Kiwix](https://kiwix.org/en/) - an offline reading app that supports wikipedia and other sites

Sound good? Let's get started.
