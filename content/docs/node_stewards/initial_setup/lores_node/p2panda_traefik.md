---
title: Opening P2Panda ports with Traefik
date: 2026-03-08T09:00:00+10:00
draft: false
type: docs
weight: 1
menus:
  docs:
    name: P2Panda traefik
    parent: LoRes Node
---

{{<hero>}}
Supporting P2Panda apps like lores-node requires a change to the default Co-op Cloud Traefik config.
{{</hero>}}

You might recall that when we setup [Accessing our Pi from the internet](../../raspberry_pi/access_from_internet), we learned about how TCP and UDP both use **ports**, and we setup port forwarding on our router for the ports we need for web traffic, and also for UDP ports `2022` and `2023` for P2Panda.

Now that we're installing **lores-node**, we need traffic to those ports to be accepted and routed by Traefik to the correct app. Even if you aren't doing port forwarding on your router (perhaps because you're just using your node on a local network), you still need to do this step.

## Edit Traefik config

It's time to edit our Traefik config with abra again:

```bash
abra app config traefik.YOUR_SERVER_DOMAIN
```

You should find a line in that file that says `P2PANDA_ENABLED=1`, but it'll be commented out (there's a hash character # in front of it). Remove the hash to enable this line.

## Re-deploy Traefik

Re-deploy Traefik with:

```bash
abra app deploy -f traefik.YOUR_SERVER_DOMAIN
```

The `-f` stands for force, and you'll need this if it's already deployed and you've made config changes.

## Push our changes to git

If we're happy that our changes to the Traefik config, we can push them to git.
