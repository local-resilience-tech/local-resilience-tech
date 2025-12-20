---
title: The Traefik web proxy
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 2
---

{{<hero>}}
We're going to have multiple apps on our Raspberry Pi, so we need a way of directing traffic to the correct one. Co-op Cloud has a standard approach to this using a web proxy called Traefik.
{{</hero>}}

Despite Traefik's special role in routing requests to other apps, it is itself just installed as a Co-op Cloud app, managed by abra.

## Creating the app

Let's add the new app using:

```bash
abra app new traefik
```

## Configuring the app

## Deploying the app
