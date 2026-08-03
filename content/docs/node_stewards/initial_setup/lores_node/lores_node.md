---
title: Installing lores-node
date: 2026-08-03T09:00:00+10:00
draft: false
type: docs
weight: 1
menus:
  docs:
    name: lores-node
    parent: LoRes Node
---

Just like with [traefik](../coop_cloud/traefik), this is installed using Co-op Cloud, but using the `abra` tool to install a new app on our server.

## Creating the app

Let's add the new app using:

```bash
abra app new lores-node --server YOUR_HOSTNAME.local --domain YOUR_HOSTNAME.local
```

Note that normally with apps on our server, we're going to put the app name in the domain, like `chat-YOUR_HOSTNAME.local`, but the lores-node app is the gateway to our server, and we're going to make it the default app by setting the app domain to the same URL as the server.

## Deploying the app

There shouldn't be any need to create custom configuration for lores-node at this stage, so we can go ahead and deploy.

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy YOUR_SERVER_DOMAIN.local --no-domain-checks
```

Then you can check that it has worked by directing a web browser at `YOUR_SERVER_DOMAIN.local`. Once again, you're going to have to do the little dance that happened with installing [Traefik](../../coop_cloud/traefik) of clicking "Advance" and "Proceed" the first time you visit this site on a new computer.

## Checklist

You should have completed:

- [x] We have created the lores-node abra app definition
- [x] We have deployed lores-node to our Raspberry Pi and can access it in the browser
