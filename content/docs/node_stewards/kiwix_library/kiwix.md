---
title: Installing the Lores-Kiwix App
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 3
type: docs
menus:
  docs:
    name: Installing Kiwix
    parent: 2. Kiwix Library
---

{{<hero>}}
[Kiwix](https://kiwix.org) is a great project to make a number of important reference websites, including Wikipedia, readable offline.
{{</hero>}}

Kiwix is available standalone on Co-op Cloud, and is you just install the regular Kiwix recipe, that will display your archives as expected. However, for **LoRes Mesh** we have provided a special [LoRes Kiwix](https://recipes.coopcloud.tech/lores-kiwix) recipe which shares information about which "books" (website archives) it has installed with other nodes in the region.

It looks a lot like installing any app with abra, so let's go through steps very similar to [other Co-op Cloud apps](../../initial_setup/coop_cloud):

## Creating the app

Let's add the new app using the command below. Remember to replace `YOUR_HOSTNAME` with the actual hostname of your Pi (eg `lores`).

```bash
abra app new lores-kiwix --server YOUR_HOSTNAME.local --domain kiwix-YOUR_HOSTNAME.local
```

Also note that although the recipe is called `lores-kiwix` we recommend just using `kiwix` in the `--domain` as above.

## Configuring the app

We do have something to configure here, so let's run:

```bash
abra app config kiwix-YOUR_HOSTNAME.local
```

There's a line in the config that starts with: `ZIM_FILES_PATH=`. We need to change that to the path to the flash drive we set in the [last section](../mounting_flash_drive):

```env
ZIM_FILES_PATH=/mnt/zims
```

And another line that starts with: `PANDA_GRPC_ADDR=`. We need to change this to:

```env
PANDA_GRPC_ADDR=http://YOUR_HOSTNAME_local_app:50051
```

## Deploying the app

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy kiwix-YOUR_HOSTNAME.local
```

Then you can check that it has worked by directing a web browser at `https://kiwix-YOUR_HOSTNAME.local` (eg, [kiwix-lores.local](http://kiwix-lores.local))

## Enjoying local Wikipedia

Now, you should have a local copy of Kiwix serving up the websites that you put on the flash drive.

You now how a local resource library in your building. If you're the only LoRes Node you know, you can stop here, but if you are connected to any others, in the next step we'll cover sharing your Kiwix "books" with the rest of your Region.

## Checklist

You should have completed:

- [x] Kiwix is running on you Pi, accessible on the local network
- [x] Kiwix is displaying the zims from your flash drive
