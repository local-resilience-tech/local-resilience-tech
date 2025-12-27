---
title: Preparing a flash drive full of "zim" files
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 3
---

{{<hero>}}
If we're going to store a local copy of Wikipedia, we're going to need some storage for it.
{{</hero>}}

Wikipedia is over 100GB, for just the English version. The MicroSD Card that [you created](../../raspberry_pi/sd_card_imaging) for the Raspberry Pi is likely not big enough to store it, an in fact we're not generally going to use that SD card for storing application data for a number of reasons. It's mostly just for the operating system.

Raspberry Pi storage can be expanded by plugging any storage device into one of it's [USB-A](https://en.wikipedia.org/wiki/USB) ports. That could be a flash drive, or a full sized hard disk. Our plan here for Wikipedia is to use a a [USB flash drive](https://en.wikipedia.org/wiki/USB_flash_drive)) (sometimes called a thumb drive) that we prepare using our **dev computer** and just plug in.

{{<aside>}}

### Flash drive size

At the time of writing, the Kiwix download of English Language Wikipedia (with images), is 111GB. There's a mini version with shortened articles and no images for 11GB. So, you can adapt the instructions here for whatever size of flash drive you have, just pick and choose the zim files that fit. However, we recommend getting a 256GB flash drive (which is the biggest size we're seeing in stores at the moment) if you can.
{{</aside>}}

If you're part of a local group running a **LoRes Mesh**, you might want to prepare several of these in advance, and hand them out to the **Node Stewards** of each node. You could have a process for updating them every three months or so.

## Formatting your flash drive

Your flash drive almost certainly came ready to use with the FAT32 filesystem, which is preferred by Windows, and supported (tolerated) by Linux and macOS.

{{<aside>}}

### Filesystems

Storage devices just store binary data, 0s and 1s. For a computer to make sense of that as been the files and directories that we expect when we examine a disk's contents, there needs to be some sort of [filesystem](https://en.wikipedia.org/wiki/File_system). Different file systems have different properties, and are preferred by different operating system.
{{</aside>}}

That isn't the best filesystem for our purposes though, we'd like to use [ext4](https://en.wikipedia.org/wiki/Ext4). Plug the flash drive into the USB port of you dev computer, and let's get formatting.

There are lots of ways to format a drive in Linux, but if you're using Ubuntu, you can launch the app called "Disks", which is actually the [Gnome Disk Utility](https://apps.gnome.org/DiskUtility/). THis should show your flash drive on the left (make sure you select the correct drive) which likely has a single big partition of the FAT32 filesystem, such as in the image below.

{{< figure src="/images/node_stewards/flash-drive-pre-format.png" alt="A screenshot of a program showing disks on the left, with the selected disk being entitled 250 GB Drive. The main part of the windows shows a set of volumes, which is just a single selected area saying - Filesystem Partition 1 250 GB FAT" caption="Gnome Disk Utility showing a 250GB flash drive with it's default FAT32 partition">}}

If you've already used it, it may have other partitions. In either case, clock on the red `-` (minus sign) button and delete this partition. Now your volumes display should list 250GB free space.

Press the `+` (plus symbol) button to create a new partition, make it the full size of the free space. On the following format screen (below), let's give the volume the name zims (because we're going to fill it with the "zim" files used by Kiwix), and chose the "ext4" filesystem as the type. Then go ahead and press create.

{{< figure src="/images/node_stewards/flash-drive-format-volume-screen.png" alt="A screenshot of a program showing a form to fill in with the fields volume name, erase and type, and a green button called Create" caption="Gnome Disk Utility format volume screen">}}

## Downloading Zim files

Kiwix works by packaging up all the text, images and other content that makes up a website into a single archive file that ends with the `.zim` extension. The Kiwix folks regularly create new versions of these files for the sites they support.

You can browse all the available zim files on the [Kiwix library page](https://library.kiwix.org/). Choose whichever files you like, perhaps including the wikipedia all articles file for your chosen language.

Once you find a file you want, you can click the "Download" button. A pop-up will appear where you can choose "Direct" to download the file directly from their server. If you're familiar with BitTorrent though, it's more community friendly behaviour to use that, perhaps by clicking the "Magnet link" option and using your BitTorrent client of your choice, such as [Transmission](https://transmissionbt.com/).

Once you've done that, copy these zim files onto your flash drive. Don't bother creating any directories or anything, just put the individual zim files directly onto the drive.

You can choose however many zims you want, but ensure that there is at least one of them on the drive before moving on to the next step.
