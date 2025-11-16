---
title: App local network URLS
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 3
type: roadmap
summary: Apps are available on the local network on a default URL that doesn't go via the internet
params:
  status:
---

{{< user_story >}}
_As a_ **User on the local network**
_I want to be able to_ **access an app in my browswer over the wifi, even if the internet is down**
_So that I can_ **rely on apps to provide me with resilient features**
{{< /user_story >}}

Our early goals for **LoRes Apps** all relate to offline copies of useful data, such as a mirror of wikipedia. Since these sites already exist online, it's more important that they are available on the local network than anything else. This means, for example, that if the app is running on a Node in your house, and you have power (so your computer is working, the node is running, and your wifi is up) but you don't have internet for some reason - you should be able to go to a URL in your browser and see the content provided by the app.

## Technical ideas

Typically local networks do not run their own DNS servers, but rely on their internet gateway for this. We want node operation to be relatively simple, with the complexity sitting mostly in **Lores Node** and not too much complexity needed in terms of special routers or configuration. Given that, we may want to rely predominantly on [Multicast DNS](https://en.wikipedia.org/wiki/Multicast_DNS) (mDNS for short) for this.

Most home networks already support mDNS by default, which means that if a computer on your network has the hostname `beetroot` then you can access it at the URL `beetroot.local`. This gives us an easy way of having a base URL for the Lores Node. However, mDNS does not support further subdomains out of the box. So `app1.beetroot.local` is probably not going to work (this isn't totally ruled out, if you're working on this task please research).

Likely, app URLS are going to be of the form `beetroot.local/app1`. This is fine, as long as our internal traffic routing provides the apps with the path they need, the base path there should be minimally intrusive.

## What success looks like

- With a node running on a local network, there should be a url that routes to the node manager, and to each app that wants a URL
- Internal links inside an app should work fine
- This should work well on most local networks without much setup
- This should work when the internet is down (eg, modem unplugged)
- App URLS should either be automatic, or documentation should exist for app developers
