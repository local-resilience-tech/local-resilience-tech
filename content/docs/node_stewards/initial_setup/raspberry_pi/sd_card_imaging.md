---
title: Custom SD card imaging
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 2
---

{{< hero >}}
Here we'll setup an SD card ready to run your Raspberry Pi the way we want. It'll contain the specific details for your login, your wifi, timezone, and so on.
{{< /hero >}}

So the MicroSD card that you're going to insert into a Raspberry Pi needs to be loaded with the operating system that will run the Pi.

We're going to use Ubuntu Linux for this.

{{< aside >}}

### Why Ubuntu?

Any Linux distribution will probably work for this step. However, for some Node Stewards, this might be their first experience using Linux. Ubuntu is a good beginner-friendly distribution. While there's not much on the Pi that is going to be distribution specific, new Node Stewards will probably also want to try out Linux on their computer (Ubuntu is a good fit for new desktop Linux users) and they will have an easier learning experience if both platforms are the same.

If you're an advances user and would prefer your distribution of choice, be aware that these instructions rely on [cloud-init](https://cloud-init.io/) which comes with Ubuntu but is likely available for your distro of choice. Also keep in mind that whatever distro you choose, it _might_ make sense to standardise within your local group to allow Node Stewards to support each other.
{{< /aside >}}

## The Raspberry Pi Imager

To install an operating system for the Pi onto a MicroSD card, you need some software called the Raspberry Pi Imager. You can download it [on this page](https://www.raspberrypi.com/software/) (for Linux, Windows or macOS) — and you install it on your **dev computer** (or in fact any computer with access to a MicroSD card reader).

These instructions assume version 2.0.0 of the Imager. If you installed an older version (perhaps by installing it via a package manager), it's worth making sure you grab the latest version from the install page.

## Using the Imager

The imager has various setup steps, see the instructions below for each step.

### Device

Make sure you select the correct Raspberry Pi model, eg the "Raspberry Pi 5". Press next.

### OS

We're going to select an operating system here. The imager presents the Raspberry Pi OS as the first choice, which is **not** what we want. Instead, you'll need to scroll down and select "Other general-purpose OS".

Inside this section, choose "Ubuntu".

Inside this section, scroll down and select the first option that contains that words _"Ubuntu Server"_, _"LTS"_ and _"64-bit"_. For example, `Ubuntu Server 24.04.3 LTS (64-bit)`.

With that done, press next.

{{< aside >}}

### An aside on Ubuntu releases

Ubuntu makes regular releases of new versions of the operating system. For all our work on building a **LoRes Mesh**, we want to use releases that are reliable and still receiving bug-fixes and security updates. The best way to do this is to only use **LTS** (Long Term Support) releases, which occur every two years and come with five years of support.

Interim releases of Ubuntu occur every six months, but only get nine months of support. These are fine (and fun) to use on your computer, but do not use them for any server infrastructure.
{{< /aside >}}

### Storage

Put the SD Card into your card reader, and on this screen select the SD Card you’ve just inserted. Ensure you have the correct device, as it’ll be completely wiped and you’ll lose anything on it. The imager program will exclude your internal hard disks from the list of options, so it's not likely you'll make a mistake here.

### Customisation

This section has a bunch of setup options which can be pretty helpful for setting up a new Pi. These write to text files used to configure _cloud-init_, a tool that sets up servers. A more advanced technique would be to write to these files directly, but for now let's use the options provider by the imager.

Fill out each of the sections below, and then hit "Next" to move on.

#### Hostname

You need to choose a hostname for this Pi. It should be short, and lower case, but also unique on your local network. Unless you're in a workshop or something where multiple people are setting up a **LoRes Node**, I'd recommend that you call it `lores`. If you've got multiple LoRes Nodes on the same location, you'll need to give them more unique hostnames, like `lores-testing` or something.

#### Localisation

Localisation asks you to fill out your:

- **Capital city** (and Country), eg: _Canberra (Australia)_
- **Time zone**, eg: _Australia/Melbourne_
- **Keyboard layout**, eg: _au_

#### User

So back on the [Login details](../login_details) page we discussed a username and password, here's where you pop them in.

#### Wi-Fi

Put in the SSID and password for your Wi-Fi network. On many platforms the Raspberry Pi Imager is able to detect this automatically and pop it in for you.

In [Login details](../login_details) we also setup a SSH key-pair, and so we're going to use that. Select "Use public key authentication". For public key, browse to the public key file that you created. On Linux it is generally found at `/home/USERNAME/.ssh/id_ed22519.pub`. Remember that it's the **pub** file that you want here. Either select the file in the browse box, or copy the files content and paste it in.

#### Remote access

We're definitely going to log in using SSH, so toggle "Enable SSH" to on.

### Write image

{{< image-section side="right" image="/images/node_stewards/raspberry-pi-imager-writing.png" width="400" alt="A picture of the Raspberry Pi Imager program, on the writing step, showing a summary of what is about to be written that matches the above steps." >}}

This screen presents a summary of what you've chosen. It should look something like this. If you're happy, go ahead and press write.

There may be an additional dialog warning you that data will be erased, and you need to press "I understand, erase and write".

The SD Card is now being prepared. You make way to make a coffee or something, this takes a little time.

{{< /image-section >}}

When it's done, remove the SD card and close Raspberry Pi Imager.
