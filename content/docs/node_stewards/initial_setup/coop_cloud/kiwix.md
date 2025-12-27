---
title: The Kiwix wikipedia server
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 4
---

{{<hero>}}
[Kiwix](https://kiwix.org) is a great project to make a number of important reference websites, including Wikipedia, readable offline.
{{</hero>}}

It looks a lot like installing any app with abra, so let's go through steps very similar to the ones we just followed with [traefik](../traefik):

## Creating the app

Let's add the new app using:

```bash
abra app new kiwix
```

## Configuring the app

Nothing to configure just yet.

## Deploying the app

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy kiwix.YOUR_SERVER_DOMAIN
```

Then you can check that it has worked by directing a web browser at `https://kiwix.YOUR_SERVER_DOMAIN`

## Pushing our changes to git

Just like with [traefik](.../traefik), if we're happy that our changes worked we can push them to git.
