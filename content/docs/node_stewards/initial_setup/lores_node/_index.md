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
We have a [Raspberry Pi](../raspberry_pi), serving wikipedia the internet with [Co-op Cloud](../coop_cloud), but we need a way to link this up with our neighbourhood so that we can start to build a local resilient mesh that supports our community.
{{</hero>}}

With do this by installing the [lores-node](https://recipes.coopcloud.tech/lores-node) software, which is a web app designed to allow Node Stewards to administer the node, and to provide some transparency to members of the public.

Just like with [traefik](../coop_cloud/traefik) and [kiwix](../coop_cloud/kiwix), this is installed using Co-op Cloud, but using the `abra` tool to install a new app on our serve.

## Creating the app

Let's add the new app using:

```bash
abra app new lores-node
```

There shouldn't be any need to create custom configuration for lores-node at this stage.

## Deploying the app

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy lores-node.YOUR_SERVER_DOMAIN
```

Then you can check that it has worked by directing a web browser at `https://lores-node.YOUR_SERVER_DOMAIN`

## Pushing our changes to git

If we're happy that our changes worked we can push them to git.
