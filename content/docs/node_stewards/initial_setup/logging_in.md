---
title: Logging in and initial setup
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 4
---

{{< hero >}}
Let's fire up our Raspberry Pi, log in using ssh, and make sure it's ready to go.
{{< /hero >}}

## Power up the Pi

Put the SD Card that you [just setup](../sd_card_imaging) into the little slot on the end of the Raspberry Pi, and plug a power supply into the USB-C slot on the Pi.

It's little light should flicker red for a bit, and then when it's working, turn green.

## Finding the Pi on your network

Since your Pi should already have your Wi-Fi credential, it will have connected to that network too. Make sure that you dev computer is connected to the same Wi-Fi network, and then we can check that the Pi is available at the expected hostname. In a terminal, try:

```bash
ping lores.local
```

If your Pi's hostname isn't `lores`, replace it with whatever hostname you chose, followed by `.local`.

If that worked, you should get a message back like:

```bash
PING lores.local (192.168.196.101) 56(84) bytes of data.
64 bytes from 192.168.196.101: icmp_seq=1 ttl=64 time=19.6 ms
```

That message shows that it was able to get an IP address from that hostname (in this case it was `192.168.196.101` but yours will probably be different).

{{<aside>}}

### How does this work?

Generally speaking, to go from a hostname to the actual IP address of a computer, your computer needs to use the [Domain Name System (DNS)](https://en.wikipedia.org/wiki/Domain_Name_System). This is a set of servers on the internet that know about registered domain names and what computers to talk to for them.

On your local network though, almost all Wi-Fi routers support [Multicast DNS (or mDNS)](https://en.wikipedia.org/wiki/Multicast_DNS). This is a simpler DNS alternative setup that lets you find named computers on your local network. For it to work with a Linux machine like our Pi, a service called `avahi-daemon` needs to be running. This is automatically installed in the `cloud-init` setup provided by the Raspberry Pi Imager.
{{</aside>}}

## Connecting via SSH

Generally speaking, if you've setup your SSH keys correctly in [this step](../login_details), including adding them to the `ssh-agent`, then from a terminal you should only need to type:

```bash
ssh lores.local
```

That's assuming your username on your dev computer is the same as on your pi. If it's different, you'll need to specify it, like `ssh aisha@lores.local`. If it's the first time you're using your ssh key in the current session, you'll also need to supply your passphrase.

The first time you SSH into a new server, you computer needs to ask you if it's legit. It'll ask you a question like:

```bash
The authenticity of host 'lores.local (192.168.196.101)' can't be established.
ED25519 key fingerprint is SHA256:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type yes and hit ENTER, and this Pi will be added to your dev computer's list of known hosts and you wont be asked that again. Next time you'll be straight in.

## Once you're in

When you SSH in successfully, you should see a detailed "Welcome to Ubuntu" message that contains useful info on the machine. If you're not using a proper power supply, for example if you're just powering the USB cable off your laptop like I am right now, you'll see a message like _"This power supply is not capable of supplying 5A; power to peripherals will be restricted"_.

Importantly, you'll almost certainly see a message about software updates, something like:

```bash
119 updates can be applied immediately.
54 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable
```

## Installing updates

This should be the first thing you do. Ubuntu Linux installs software updates using a tool called `apt`. You can read more details about this [here](https://documentation.ubuntu.com/server/how-to/software/package-management/), but essentially to install these upgrades there are two steps.

{{<aside>}}
Both of these steps require the command `sudo`, which means "super-user do [the following command]". The first time you use sudo, you'll have to enter the password for your raspberry Pi account that you set in the [login details](../login_details) step. This is not your ssh key passphrase, but the password you setup for this specific Pi.
{{</aside>}}

1. Update to the latest package lists so you the system knows what can be upgraded by running `sudo apt update`.
2. Upgrade your installed software to the latest version by running `sudo apt upgrade`. This step will likely prompt you to continue (hit ENTER or Y for yes). It might also take a few minutes.

Very occasionally, upgrades require a restart of the Pi to take full effect. For example, updates to the Linux Kernel itself will usually output the message _"Restarting the system to load the new kernel will not be handled automatically, so you should consider rebooting."_. You can check if it does but running `cat /var/run/reboot-required`.

If it needs a reboot, run `sudo reboot`, wait a minute or so, and then you can SSH back in.

Further updates should happen automatically, because the `unattended-upgrades` package is installed by default.

## Our Pi is ready for action

Now we have a Pi ready to setup as a **LoRes Node**. In the next section we'll cover how to start using Co-op Cloud, a system for installing open source web software.
