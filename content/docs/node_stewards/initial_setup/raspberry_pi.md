---
title: Preparing your Raspberry Pi
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 1
---

## What is a Raspberry Pi 5

A Raspberry Pi is just a small computer, designed for hobby and education uses, but with many other useful applications. There are different versions of the Pi, and we're going to assume the [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/).

A Pi doesn't come with a monitor, keyboard or mouse, although it does have ports for those to plug into. It also doesn't come with a hard drive built it, but it has a slot for a [micro SD Card](https://en.wikipedia.org/wiki/SD_card) which it uses as it's main working storage, and USB ports for plugging in external storage. It does have RAM for working memory, which comes in a variety of sizes that influence cost. Currently available are 2GB, 4GB, 8GB, 16GB versions. Our testing for these docs has been done with 8GB, so that's what we're recommending.

Importantly, the processor in a PI uses an architecture called [ARM](https://en.wikipedia.org/wiki/ARM_architecture_family). This type of processor is less common than the [x86](https://en.wikipedia.org/wiki/X86) architecture used in most home computers. The advantage of ARM is that it's much lower power, and so you almost certainly have an ARM processor in your phone, and maybe in your laptop. This is one of the main reasons we're using the Pi for our **LoRes Node**. It's low power needs means that it draws a maximum of 25W of power (with all accessories powered at peak use). The downside of this to watch out for is that not all software supports ARM, which is something we'll guide you through as part of this project.

## Putting your Pi together

Your Pi typically comes as a loose board, a power supply, and a case you buy separately. If you're not sure which case to buy, start with the [official case](https://www.raspberrypi.com/products/raspberry-pi-5-case/), as it has a cooling fan.

Assembling your Pi is just a matter of following the [instructions that come with the case](https://datasheets.raspberrypi.com/case/case-for-raspberry-pi-5-product-brief.pdf). Regardless of which case, this usually includes sticking a heat-sink on the CPU, and for active cooling cases like the official one, plugging in the fan.

There's no point in powering it up yet, but when you're ready for that, your power supply simply plugs into the only [USB-C](https://en.wikipedia.org/wiki/USB-C) port on the Pi. Note that it is probably best to grab an official Raspberry Pi power supply for the Pi 5, because not all USB power supplies are capable of outputting 25W.

## Next, operating system

The Pi's operating system is stored on the MicroSD card and inserted into the card slot (at the opposite end of the Pi from all the USB ports). In the next section we'll prepare that MicroSD card for use.
