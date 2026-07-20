---
title: Join or start WiFi
date: 2026-07-20T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: The Raspberry Pi will automatically start a WiFi access point if it can't succesfully join one
params:
  status: in-progress
  assigned:
    - username: mattcen
      role: Development
---

{{< user_story >}}
_As a_ **User on the local network**
_I want to be able to_ **access the LoRes Node even if the building wifi goes down**
_So that I can_ **can use the apps in during an outage**
{{< /user_story >}}

If the Raspberry Pi is able to access and log into a wireless network, then everything is good. It's probably going to be better and more reliable to use that. And, if we don't want it on a wireless network, we simply don't give it the login details

However, if the Pi notices that the network has gone down, it should automatically start operating as it's own access point. Ideally that should have a name derived from it's hostname.

It should make this decision on boot, and also if it's situation changes, such as the WLAN that it normally joins being down for five minutes.
