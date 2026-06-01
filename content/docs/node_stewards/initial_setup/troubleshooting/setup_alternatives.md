---
title: Initial Login & Setup Alternatives
date: 2026-05-28T09:00:00+10:00
draft: false
weight: 1
type: docs
menus:
  docs:
    name: Setup Alternatives
    parent: Troubleshooting
---

{{<hero>}}
Sometimes setup doesn't go to plan..
{{</hero>}}

There can be lots of reasons for this, and the Raspberry Pi documentation includes [troubleshooting advice](https://www.raspberrypi.com/documentation/computers/getting-started.html#troubleshooting).

The following is an in-progress collection of alternative approaches that may be useful to know about during the setp stage if running into troubleshooting issues.

If you can't find the issue you've encountered in the troubleshooting docs, please reach out for help.

If you have an alternative you'd like to share and are comfortable submitting a pull-request, adding it to the list below will help others along the way.

## Alternative to using the rpi-imager tool

If the right ssh private key can't be found, one option is to take the SD card out, insert (mount) it in a (Linux) laptop, and append a new key to the authorised_keys file to give ourselves access. (This is possible because the Pi's SD card is unencrypted)
