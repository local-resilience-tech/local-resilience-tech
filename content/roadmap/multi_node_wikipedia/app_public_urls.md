---
title: App public URLS
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 4
type: roadmap
summary: Apps are available on the public internet at a URL per app
params:
  status:
---

{{< user_story >}}
_As a_ **LoRes Mesh stakeholder**
_I want to be able to_ **access an app in my browswer over the internet, at a URL that directs me to a specific app and node**
_So that I can_ **begin to see how nodes can provide me with digital services in an ongoing way**
{{< /user_story >}}

Early **LoRes Apps** features all relate to offline copies of useful data, such as a mirror of wikipedia. Since these sites already exist online, accessing them at a URL over the internet isn't terribly useful for genuine user features.

However, we are building this collectively and we need to be able to demonstrate our work to each other. So, this feature is primary designed to be used to provide early demonstration of LoRes App features to key stakeholders, such as other developers, or volunteers and partners at local groups using **LoRes Mesh**, such as [Merri-bek Tech](https://www.merri-bek.tech/).

## Technical ideas

This feature is going to require real DNS. At this stage it is suggested that apps use subdomains for this, and DNS is going setup to point a wildcard subdomain to your node. It's also possible to consider using paths instead of subdomains.

If your **Lores Node** is running on a home internet connection, you may need to setup port mapping on your router if it supports it, and you may need a static IP address. It's unclear what we can do to support situations where these are not available. We do not generally want traffic to have to go via an internet proxy if we can avoid it, although that may be an option for testing purposes.

## What success looks like

- A specific URL should work to access the app from the internet
- Internet access to the Lores Node manager app should be possible but not required
- App URLS should either be automatic, or documentation should exist for app developers
