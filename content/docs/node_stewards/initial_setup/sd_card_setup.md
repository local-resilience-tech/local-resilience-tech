---
title: Generic SD Card Setup
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
---

# Preparing a Generic SD Card

This document describes how to prepare an SD Card to boot a Raspberry Pi to be
used in a site swarm, or for experimental purposes. Completing these steps
produces an SD Card that has all the general configuration, but will have a
default hostname, and won't have the login details for your local WiFi.

Since there's nothing site-specific about the SD Card prepared here, you could
prepare a number of them and hand to other volunteers for site-specific setup.

## Initial SD card operating system installation

1. Download [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
2. Start up Raspberry Pi Imager and use "CHOOSE DEVICE" to select your
   Raspberry Pi model
3. Select "CHOOSE OS" and navigate to
   "Other general-purpose OS" -> Ubuntu -> "Ubuntu Server 24.04.1 LTS (64-bit)"

   NOTE: If you are using a Raspberry Pi 2 or below, or a Raspberry Pi Zero,
   you'll need to pick the 32-bit version instead, and some apps may take some
   effort to get working

4. Insert your SD Card into your computer
5. Select "CHOOSE STORAGE" and select the SD Card you've just inserted. Ensure
   you have the correct device, as it'll be completely wiped and you'll lose
   anything on it
6. Select "NEXT"
7. When asked "Would you like to apply OS customisation settings?", Select "NO"
8. You'll now receive your final prompt to confirm this is the correct storage
   device to wipe. Check this, and then select "YES"
9. When prompted, remove the SD card and close Raspberry Pi Imager

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
