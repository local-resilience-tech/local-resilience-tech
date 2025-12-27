---
title: Accessing the flash drive on with the Pi
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 4
---

{{<hero>}}
When you put flash drive we just [prepared](../preparing_flash_drive) in your dev computer, it probably pops up ready to read in your file manager. The Raspberry Pi won't do that automatically, so we need to set that up.
{{</hero>}}

## Plugging it in

Let's start by plugging the flash drive into one of the USB ports on the Pi. On a Raspberry Pi 5, only two blue USB (type A) ports support the faster USB-3. Providing you have a modern flash drive that supports it, you want to put it in one of those.

{{<aside>}}

### Mounting a drive

Making the contents of the drive accessible to the Pi's operating system is called [mounting](<https://en.wikipedia.org/wiki/Mount_(computing)>). When the drive is mounted, there's a path you can go to in your terminal to see the files, often something like `/mnt/zims` or `/media/zims`. Those are just conventions though, it could actually be any path.

Here's a [longer tutorial](https://raspberrytips.com/mount-usb-drive-ubuntu-server/) on how to mount a flash drive on a Pi running Ubuntu Server, but the instructions below are the path we recommend.
{{</aside>}}

All the commands for this page are run on the command line while logged into the Raspberry Pi. So, first log in using `ssh lores.local` (or whatever your hostname is).

## Setting up auto-mounting

We don't just want to mount our flash drive once, we want it to automatically mount even if the Raspberry Pi resets. We'll do that by adding an entry for the flash drive to the [fstab](https://en.wikipedia.org/wiki/Fstab) file, which controls automatic mounting.

### Getting the drive UUID

There's a unique identifier, or [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier), for each each drive we plug in. To find the one for the flash drive you just plugged into the pi, run:

```bash
blkid
```

That'll return a bunch of text, like this:

```bash
/dev/mmcblk0p1: LABEL_FATBOOT="system-boot" LABEL="system-boot" UUID="8AA8-96C7" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="841506be-01"
/dev/mmcblk0p2: LABEL="writable" UUID="9276ecfd-6dd5-4e22-9a91-2afafd0a53a3" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="841506be-02"
/dev/loop1: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop0: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/sda1: LABEL="zims" UUID="c5272036-0e87-423b-b2fc-9c99e03c303b" BLOCK_SIZE="4096" TYPE="ext4"
```

Each line of this is a different device. There are a bunch of clues that might help us figure out which one is the thumb drive we just plugged in. Perhaps the best clue is the `LABEL="zims"`, which is the name we gave our flash drive when [preparing it](../preparing_flash_drive). Also `/dev/sda1` is the most common name for the first plugged in disk.

So given that, the UUID we want is the one for that line, in this example it's `c5272036-0e87-423b-b2fc-9c99e03c303b`.

### Creating the mount point

You need to create a folder for the flash drive to be mounted at, lets do that with:

```bash
sudo mkdir /mnt/zims
```

### Editing the fstab file

We're going to use the `nano` text editor to edit this file, and also we need super user access to do this, so we're going to use the sudo command.

```bash
sudo nano /etc/fstab
```

Add the following line to the end of the file, replacing the UUID with the one for your device:

```bash
UUID=c5272036-0e87-423b-b2fc-9c99e03c303b /mnt/zims ext4    defaults        0 0
```

### Trying the mount

This should work automatically when the Pi is reset, but an easier way to try it out now is to run:

```bash
mount -a
```

Don't worry if you get _"(hint) your fstab has been modified, but systemd still uses the old version)"_. Providing there are no other errors, you should be good to go. You can check if your files are there using:

```bash
sudo ls /mnt/zims
```

You should see a list of all those zim files you put on the flash drive. If you want to be extra sure it's going to stay working, you could try rebooting the Pi (`sudo reboot`) and then login and run that `sudo ls /mnt/zims` command again and it will still work.

Ok, we've got everything ready to serve up Wikipedia now, let's wire it all together in the next step.
