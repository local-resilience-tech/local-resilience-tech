---
title: "Part 2: Apps managed with Co-op cloud"
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 2
type: docs
menus:
  docs:
    name: Co-op Cloud
    parent: Initial setup
---

{{<hero>}}
We have a [Raspberry Pi](../raspberry_pi) ready to be our server, in this section we'll use **Co-op Cloud** to install the **Traefik** app, which direct traffic to all our other apps.
{{</hero>}}

Co-op Cloud consists of a set of [recipes](https://recipes.coopcloud.tech/) for different apps, and a command line tool called [abra](https://docs.coopcloud.tech/abra/) which is used to configure and deploy them to servers.

Our instructions here follow a similar path to the Co-op Cloud [New operators tutorial](https://docs.coopcloud.tech/operators/tutorial/), but with more opinionated choices made for our setup.

...let's get started
