---
title: Local-first setup
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: Setting up a node starts on the LAN and can be tested locally
params:
  status: in-progress
  assigned:
    - username: jade
      role: Development
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **setup my pi and see it working without having to configure my router, get an IP or a domain name**
_So that we can_ **avoid obstacles to learning and success**
{{< /user_story >}}

This requires a landing page that works over mDNS.

## Key Risk

Can we use HTTPS for a domain like `lores.local`. If not, will common browsers accept an HTTP connection instead.

## Resources and notes

- Default Raspberry Pi ubuntu config does not publish avahi record. The fix is to enable `publish-workstation` and restart the avahi daemon as per [this article](https://askubuntu.com/questions/1458161/avahi-browse-cannot-find-all-local-computers-although-mdns-is-working)
- This go program is amazing, and does what it says on the tin. [go-avahi-cname](https://github.com/grishy/go-avahi-cname)

So the `go-avahi-cname` app uses CNAME mDNS records (as you might expect). When I run that, I can detect the subdomain from my dev machine with a command like `avahi-resolve-host-name traefik.radish.local`.

However, if I try and ping, I get the following results on different platforms.

- MacOS: Works fine
- Linux (Ubuntu 26.04 LTS): Does not work. Instructions to fix it require a change to the client computer, which is annoying
- Windows: Does not work

### Fixing on Ubuntu

`/etc/nsswitch.conf` — change `mdns4_minimal` to `mdns`

Create `/etc/mdns.allow` containing:

```txt
.local.
.local
*.local.
*.local
```

### Switching to mDNS A records

The above approach has two problems. Firstly, mDNS CNAME records aren't really a thing, they're an Avahi extension, not supported by all mDNS resolvers.

Secondly, not all platforms support the approach of nesting "subdomains" in mDNS.

The solution is to use A records, and to format them like `traefik-radish.local` (or perhaps `radish-traefik.local`).

To test this, run a command like the following on the pi

```bash
avahi-publish-address -R traefik-radish.local 192.168.196.232
```

Where the local IP of the pi is used, or find that dynamically using:

```bash
avahi-publish-address -R traefik-radish.local $(hostname -I | awk '{print $1}')
```

#### Productionising the A-record approach

There's no well-maintained tool that publishes multiple A records (not CNAMEs) via Avahi dynamically. The ecosystem has mostly converged on CNAMEs for this use case, which is why cross-platform support is patchy.

Either I need to run multiple avahi-publish-address processes, or write a small program to call the Avahi D-Bus API to publish multiple A records in one process.
