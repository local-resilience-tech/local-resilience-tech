---
title: Setting up a Firewall
date: 2026-05-25T09:00:00+10:00
draft: false
weight: 5
type: docs
menus:
  docs:
    name: Firewall
    parent: Raspberry Pi
---

{{<hero>}}
Let's lock our server down so that no one can access services we aren't meaning to expose.
{{</hero>}}

A [firewall](<https://en.wikipedia.org/wiki/Firewall_(computing)>) is software that allows or prevents incoming and outgoing network traffic on a machine. For many of us, firewalls were often a black art, more likely to cause problems than prevent them. _Got a problem on your machine? Try turning off the firewall._

[Uncomplicated Firewall](https://wiki.ubuntu.com/UncomplicatedFirewall) (or `ufw`) makes it simple enough that adding this extra layer of protection is easy to do, no matter what your skill level. This step is optional, but highly recommended. The `ufw` tool comes installed by default on Ubuntu Linux. This article is based on this excellent [Uncomplicated Firewall Reference](https://linuxize.com/post/ufw-command-in-linux/) article by Dejan Panovski.

The following instructions are all run **on your Raspberry Pi**, so go ahead and ssh in.

## Initial status

Check that the ufw is installed, but inactive. Run:

```bash
sudo ufw status
```

You should see:

```bash
Status: inactive
```

## Default policies

For protection by default, let's tell the firewall to prevent all incoming traffic, and allow outgoing.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

## Allow ssh traffic

If we turned the firewall on it with those defaults, we'd be locked out of ssh, so before you do anything else, run:

```bash
sudo ufw limit ssh
```

This allows ssh connection, but rate-limited, to help prevent brute-force attacks.

## Allow our lores traffic

Let's enable the web and P2Panda traffic that we need for our LoRes Node.

```bash
sudo ufw allow http
sudo ufw allow https
sudo ufw allow 2022/udp
sudo ufw allow 2023/udp
```

## And turn it on

Turn on the firewall with:

```bash
sudo ufw enable
```

And that's it. Once it's it's enabled, you can always check your status with `sudo ufw status`.

## Checklist

You should have completed:

- [x] Enabled ufw on your Raspberry Pi
