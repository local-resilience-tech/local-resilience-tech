---
title: Changing WiFi details
date: 2026-05-28T09:00:00+10:00
draft: false
weight: 2
type: docs
menus:
  docs:
    name: Changing Wifi
    parent: Troubleshooting
---

{{<hero>}}
If you're using a Raspberry Pi without a screen, it's impossible to log-in if you can't get it on the network, which can make changing location really hard.
{{</hero>}}

Luckily, there's a way to put in a new WiFi password without having a screen or keyboard attached to the Pi. Instead you, need an SD card reader.

## Mount the SD Card on your dev computer

Take the MicroSD card out of the Pi and put it in the card reader attached to your dev computer.

{{<aside>}}

### Removing the card

Sometimes the SD card can be hard to remove from the Pi. We've found that the best approach if you can't manage to pull it out with your fingernails, is to use a small pair of needle nose pliers. Grip the end of the SD card. If it doesn't come out with a firm but gentle pull, don't yank it (you can break them that way). Instead, give it a slight gently wiggle from side to side and pull.
{{</aside>}}

Then, pop it in your computer and it should automatically "mount", and appear in your file browser, the same way that you'd expect from an thumb drive. That's "Files" in Gnome (used by Ubuntu Linux), Finder in MacOS, Windows Explorer etc.

When you do this, it should mount two folders for you, `system-boot` and `writable`.

## Editing network config

Inside the `system-boot` folder, you'll find a file called `network-config`. Edit this in the text editor of your choice. It's contents should look a bit like this:

```txt
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      regulatory-domain: "AU"
      access-points:
        "My Awesome WiFi":
          password: "8FFA2C40DF18020B75496602F7D10A7E7AF4C00C059E7CB811C16B7ED3B09C44"
      optional: true
```

In this file we can see nested config. The format of this file is [YAML](https://en.wikipedia.org/wiki/YAML), where each section that has been further indented (in this case by two spaces) is considered to be nested inside the previous section.

Give this, for our network config, we have a wifi interface called `wlan0` and that inside it there's a section called `access-points`. Inside that, you probably see details for one wireless network, that you configured when initially flashing the SD card. The example above has that called "My Awesome WiFi", and it has a password which has been encrypted into a hexadecimal string so we can't read it.

You can add another wifi network and password to this. It's a bit hard to encrypt the password yourself, but it still works to put it in plain text.

## Adding your own network

So, let's add another network to that.

```txt
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      regulatory-domain: "AU"
      access-points:
        "My Awesome WiFi":
          password: "8FFA2C40DF18020B75496602F7D10A7E7AF4C00C059E7CB811C16B7ED3B09C44"
        "My new house":
          password: "topsecret"
      optional: true
```

You'll see above that I added the wifi network for `My new house` with the password `top secret`.

With that done, save the file. Then un-mount the two mounts for the SD card in your file manager, pull the card out, pop it back in the Pi and boot it up. It should automatically connect to your new network.
