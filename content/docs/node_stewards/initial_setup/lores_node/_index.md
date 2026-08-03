---
title: "Part 3: LoRes Node, connecting us to other local servers"
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 3
type: docs
menus:
  docs:
    name: LoRes Node
    parent: Initial setup
---

{{<hero>}}
We have a [Raspberry Pi](../raspberry_pi), setup to run apps managed with [Co-op Cloud](../coop_cloud), but we need a way to link this up with our neighbourhood so that we can start to build a local resilient mesh that supports our community.
{{</hero>}}

We do this by installing the [lores-node](https://recipes.coopcloud.tech/lores-node) software, which is a web app designed to allow Node Stewards to administer the node, and to provide some transparency to members of the public.

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

Then you can check that it has worked by directing a web browser at `YOUR_SERVER_DOMAIN.local`. Once again, you're going to have to do the little dance that happened with installing [Traefik](../coop_cloud/traefik) of clicking "Advance" and "Proceed" the first time you visit this site on a new computer.
