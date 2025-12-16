## Preparing a Generic SD Card

See [Preparing SD Card](./docs/preparing_generic_sd_card.md) to setup a generic SD Card
for a swarm PI, unless someone else has prepared one for you.

## Customising SD Card for you Site

1. Insert the Generic SD card from the step above into a card reader
   connected to your computer.
2. Your computer should detect the card and you should be able to find a new
   drive in your file manager. On MacOS this will be called "system-boot"
3. Open the file `user-data` in a text editor and find the line
   `hostname: raspberrypi`. Change the word `raspberrypi` to a hostname
   likely to be unique on your local network. If you're setting up a site
   called `mysite`, we recommend `mysite1` as a hostname (and this will
   be used as an example in following instructions).

### Optional: Adding WiFi Credentials

This step is only needed if you want to access the Pi over WiFi. For serving web
apps, it's going to be more efficient and reliable if you can connect the Pi via
ethernet cable to your router. If you do this, WiFi credentials are not needed.

4. Open up the file `network-config` in a text editor. Find the section
   with the line `access-points:`. Underneath that, any number of WiFi
   network details can be added. Note that this file is a text file in
   the [YAML](https://en.wikipedia.org/wiki/YAML) format. This means that
   you need to indent sections inside `access-points` by two spaces
   (not a tab), and use quote marks `""` around your network name if it
   contains spaces.

## Raspberry Pi first boot

1. Insert a configured SD card into your Raspberry Pi, and connect ethernet,
   then power
2. Wait a few minutes, and then you should be able to run `ssh
pi@mysite1.local` (replace the hostname with whatever hostname you set)
   with the default password `mbt`
