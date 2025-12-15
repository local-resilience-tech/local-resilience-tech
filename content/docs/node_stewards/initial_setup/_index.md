---
title: Initial Node Setup
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 1
type: docs
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to_ **setup a node that provides a copy of wikipedia on my local network**
_So that we can_ **access useful information even if the internet goes down**
{{< /user_story >}}

## Our goals

Welcome **Node Steward**. We're going to walk through setting up your first node. Our goal is to keep this fairly simple, so this node will use simple hardware that can be expanded later, and will only have one web application installed, an offline reader for Wikipedia.

Is this useful? We think so. **LoRes Mesh** is all about providing local resilience in the face of the worsening climate crisis. Even if you live somewhere where the internet has always been reliable, storms, heat waves, fires and floods are going to be more and more likely in the coming years and this does mean you could lose internet access or power.

With that in mind, our goal is to get a working low-power computer on your local network serving up a copy of wikipedia, which you can browse to on your local network even if the internet is down.

## Non goals

To keep things quick for this initial setup, we are **not** going to:

- Power your node with a battery, so initially it will only work if the internet is down, not if you've lost power.
- Connect your node to other nodes in your local region, that comes next.
