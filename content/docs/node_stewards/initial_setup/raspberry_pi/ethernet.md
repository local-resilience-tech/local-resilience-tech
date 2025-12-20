---
title: Connecting via ethernet cable (Optional)
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 5
---

{{<hero>}}
Using your PI as a web server will go more smoothly over a wire, but it's note required if you don't have a router that supports it.
{{</hero>}}

Now is a great time to consider connecting our Raspberry Pi to our router via an [ethernet](https://en.wikipedia.org/wiki/Ethernet) cable, rather than over Wi-Fi. Many home routers support both. You can check if yours does by seeing if it has little square sockets for a [modular connector](https://en.wikipedia.org/wiki/Modular_connector) on the back. Most routers have one socket for the input line - possibly an ethernet cable to a modem, or perhaps the router is also a modem and it has some other sort of input cable (phone line, fibre cable, co-axial, etc). Often, they also have a set of output lines, maybe numbered, perhaps four or eight of them.

If you don't have a socket for an outgoing ethernet connection, you can skip this section and stick with Wi-Fi. It should work fine, but if it is something that you want to fix in the future that might mean a new router.

### Connecting the cable

If you do have an output socket, then you need to plug an ethernet cable (there are multiple types, all current ones should work, eg Cat5, Cat5e, Cat6) in to that output socket, and into the modular connector socket on the Raspberry Pi.

To see if it's working, you can ssh into the Pi and run:

```bash
ip a
```

This lists your current network devices. Ethernet ones start with `eth`, so your pi probably has `eth0` as the second one on the list, perhaps in a line like this:

```bash
2: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
```

As you can see, this is **DOWN** at present. We're not networking over ethernet because our eth0 device isn't enabled.

### Enabling eth0

To enable this, we're going to use [cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html). Cloud-init is a system for configuration at the time of provisioning (setting up) servers. If you enjoy being a **Node Steward** with this first Pi, then it's possible to use cloud-init to automate the setup of lots of Raspberry Pis.

So, let's edit the cloud-init file used for setting up the network configuration. These files are owned by root, so you'll need sudo. You'll also need to use one of the text editor commands built in, we've chosen `nano` because it's easy to use (but if you know `vi` you could use that).

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Once editing the file, you'll notice it contains some structured configuration for your networking, mostly just your Wi-Fi details. The file is in YAML format.

{{<aside>}}

### 📖 YAML

YAML (rhymes with camel) is a file format for structured data that is readable by both humans and machines. It uses indenting (white space at the start of the line) to indicate that some data is nested inside other data. It's a little picky about getting this white-space correct and consistent. You can learn more about YAML [here](https://www.yaml.info/learn/index.html).
{{</aside>}}

The top sections of the file will be `version` and `wifis`. We want to put a new section in for `ethernets`, let's insert that above the `wifis` section. Change the file so that it looks something like this (the `wifis` section shouldn't change, it's just included here to give you a complete picture):

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
      optional: true
  wifis:
    wlan0:
      optional: true
      dhcp4: true
      regulatory-domain: "AU"
      access-points:
        "YOUR SSID HERE":
          auth:
            key-management: "psk"
            password: "YOUR PASSWORD HERE"
```

You can exit nano with CONTROL-X, then it'll ask you if you want to save (hit Y), and the filename (hit ENTER to keep it the same).

Having made this change, cloud-init doesn't immediately change networking config, it only sets the actual configuration when the machine boots. So, reboot your Pi using:

```bash
sudo reboot
```

Give it a minute to book back up, then SSH in again. If you've got your ethernet cable plugged in, and you run `ip a` again, you should see the `eth0` line now contains the word `UP` instead of `DOWN`.
