---
title: Generic SD Card Setup
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
---

{{< hero >}}
Here we'll setup a **generic** SD card ready to run a Raspberry Pi the way we want. Nothing on this page is specific to your **LoRes Node**, so someone in your group could prepare multiples of these cards and hand them out. If you have been given one, skip to the next page.
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

This section has a bunch of setup options which can be pretty helpful for setting up a new Pi. Things that are chosen here are specific to a particular Pi though, such as hostname and Wi-Fi password. We're going to leave a bunch of this to setup later, but we recommend filling out just the **Localisation** section for now.

Localisation asks you to fill out your:

- **Capital city** (and Country), eg: _Canberra (Australia)_
- **Time zone**, eg: _Australia/Melbourne_
- **Keyboard layout**, eg: _au_

One particular reason why it's important to do this is that there are different standards for Wi-Fi in different countries. Presuming your group is all in the same country and timezone, setting this up still leaves your SD card able to be used amongst any of your collaborators.

So, hit "Next" on all of the steps, leaving them blank, except for "Localisation" which you fill out with your local details. Then hit "Next" to move to the next section.

### Write image

{{< image-section side="right" image="/raspberry-pi-imager-writing.png" width="400" alt="A picture of the Raspberry Pi Imager program, on the writing step, showing a summary of what is about to be written that matches the above steps." >}}

This screen presents a summary of what you've chosen. It should look something like this. If you're happy, go ahead and press write.

There may be an additional dialog warning you that data will be erased, and you need to press "I understand, erase and write".

The SD Card is now being prepared. You make way to make a coffee or something, this takes a little time.

{{< /image-section >}}

When it's done, remove the SD card and close Raspberry Pi Imager.

## Preparing the OS for LoRes Mesh

## Customising operating system configuration

1. Insert the SD card you've just freshly flashed with Ubuntu Server into a
   card reader connected to your computer.
2. Your computer should detect the card and you should be able to find a new
   drive in your file manager. On MacOS this will be called "system-boot"
3. Copy all of the files from this repository's `cloud-init` directory into the
   top directory of the SD card
4. Eject the SD card from your computer

## Checking it worked

If you want to test that this SD Card worked, you can put it in a Raspberry Pi,
plug in a monitor (you may need a micro-hdmi to hdmi adaptor), and a usb
keyboard. You should then be able to login using the username `pi` and the default
password `mbt`.

This check is not necessary if you're going to customise the setup for your local
WiFi first.
