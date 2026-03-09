---
title: Nodes advertise apps
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 8
type: roadmap
summary: A node provides a list of it's installed apps
status: done
---

{{< user_story >}}
_As a_ **Web user**
_I want to be able to_ **see what apps are available on a node**
_So that I can_ **find out of the kiwix app is present**
{{< /user_story >}}

_LoRes Node_ doesn't manage apps (that's done via Co-op Cloud) but it does need to know which ones are present. This allows it to present a list of these apps to public users and to node stewards.

This step is a necessary precursor to [Regional traffic routing](../regional_traffic_routing) because when the apps are known, they can be registered with the region, allowing for traffic to be routed to them.

## What success looks like

- [x] LoRes Node attempts to read all the docker stacks installed
- [x] Only apps that are flagged as Co-op cloud apps are listed
- [x] Each app has a link that can be followed to see it
- [x] Co-op cloud recipe connects LoRes app to the docker socket so that the above all works in production
