---
title: The Kiwix wikipedia server
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 5
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

We do have something to configure here, so let's run:

```bash
abra app config traefik.YOUR_SERVER_DOMAIN
```

There's a line in the config that will look like this:

```env
ZIM_FILES_PATH=/path/to/your/zim/files
```

We need to change that to the path to the flash drive we set in the [last section](../mounting_flash_drive):

```env
ZIM_FILES_PATH=/mnt/zims
```

## Deploying the app

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy kiwix.YOUR_SERVER_DOMAIN
```

Then you can check that it has worked by directing a web browser at `https://kiwix.YOUR_SERVER_DOMAIN`

## Pushing our changes to git

Just like with [traefik](.../traefik), if we're happy that our changes worked we can push them to git.

## Enjoying local Wikipedia

You already tried it above, but once again that kiwix server should be available at [https://kiwix.YOUR_SERVER_DOMAIN](https://kiwix.YOUR_SERVER_DOMAIN). That page should present an index of all the websites you downloaded as zim files, and let you browse them.

You now how a local resource library in your building. In the next section, we'll be setting up the **LoRes Node** software to allow your node to join others in your area, and collaborate in serving up apps such as kiwix.
