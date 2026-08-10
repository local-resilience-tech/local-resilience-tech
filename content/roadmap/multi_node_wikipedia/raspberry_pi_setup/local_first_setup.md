---
title: Local-first setup
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: Setting up a node starts on the LAN and can be tested locally
params:
  status: done
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

### mDNS A Records

I've got this working. I built a tool called [coop-cloud-mdns-publisher](https://github.com/local-resilience-tech/coop-cloud-mdns-publisher) that automatically publishes mDNS records for Co-op Cloud services.

I have the following work to do in ccmdns:

- [x] For apps with hyphens in their names: When I setup the app `lores-node-radish.local-env`, it seems to think it's app name is `lores-node-radish`, and so ccmdns creates a mDNS record called `lores-node-radish-radish.local`. _I've fixed this for now by using recipe name instead_

## App publishing

I can publish the app in abra, providing I use the `-D` / `--no-domain-checks` flag. Otherwise, it'll try and check the domain on the internet and fail.

I've found the following niggles that I'd like to fix in abra, to make things easier:

- [ ] Automatic no domain lookup on `.local` addresses
- [ ] When creating a new app for a `.local` address, auto-suggest `appname-hostname.local` instead of `appname.hostname.local`

## Self signed certificate

Instructions based on [Running an offline co-op cloud server](https://docs.coopcloud.tech/operators/handbook/#running-an-offline-coop-cloud-server)

**Step 1**: Add the wildcard config to traefik. Run `abra app config traefik-radish.local` before deploying Traefik (if already deployed, un-deploy it).

Un-comment the lines in the config:

```env
WILDCARDS_ENABLED=1
SECRET_WILDCARD_CERT_VERSION=v1
SECRET_WILDCARD_KEY_VERSION=v1
COMPOSE_FILE="$COMPOSE_FILE:compose.wildcard.yml"
```

**Step 2**: Generate a certificate for the local server:

```bash
openssl req -x509 -out radish_local.crt -keyout radish_local.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=radish.local' -extensions EXT -config <( \
   printf "[dn]\nCN=radish.local\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:radish.local,DNS:*.local\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

Note that the CN (Common Name) is my local Pi hostname (eg `radish.local`) but it's critical that there's also a subjectAltName that is `*.local` to cover the hyphenated app names, like `traefik-radish.local`.

**Step 3**: Add this certificate as docker secrets

```bash
abra app secret insert traefik-radish.local ssl_cert v1 radish_local.crt -f
abra app secret insert traefik-radish.local ssl_key v1 radish_local.key -f
```

**Step 4**: Deploy traefik

```bash
abra app deploy traefik-radish.local -D
```

{{<archive>}}

## Archived notes

- Default Raspberry Pi Ubuntu config does not publish Avahi record. The fix is to enable `publish-workstation` and restart the Avahi daemon as per [this article](https://askubuntu.com/questions/1458161/avahi-browse-cannot-find-all-local-computers-although-mdns-is-working)
- This go program is amazing, and does what it says on the tin. [go-avahi-cname](https://github.com/grishy/go-avahi-cname)

So the `go-avahi-cname` app uses CNAME mDNS records (as you might expect). When I run that, I can detect the subdomain from my dev computer with a command like `avahi-resolve-host-name traefik.radish.local`.

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

#### Production ready A-record approach

There's no well-maintained tool that publishes multiple A records (not CNAMEs) via Avahi dynamically. The ecosystem has mostly converged on CNAMEs for this use case, which is why cross-platform support is patchy.

Either I need to run multiple `avahi-publish-address` processes, or write a small program to call the Avahi D-Bus API to publish multiple A records in one process.

## App Traffic routing

The Co-op Cloud traefik setup insists on HTTPS (sensibly). I need to investigate using self-signed certificates for .local apps, or using (gasp) HTTP.

- Traefik dashboard wont display on insecure HTTP (probably for the best)
- Traefik wont direct traffic to lores-node-radish.local on insecure HTTP (sad face)

{{</archive>}}
