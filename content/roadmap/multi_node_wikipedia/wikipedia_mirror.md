---
title: Standalone Wikipedia mirror
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: Node stewards can install wikipedia onto a Raspberry Pi and serve it at a public URL
params:
  status: testing
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **serve a mirror of wikipedia to the public**
_So that I can_ **experience providing a useful web service over the internet**
{{< /user_story >}}

A **Node Steward** who has already [set-up a Raspberry Pi](../raspberry_pi_setup) can install the Kiwix app and run a mirror of Wikipedia (and other zim files if they like) using a USB flash drive for storage.

They have also been able to point a domain name at their node, by using a static IP address for their internet connection, and setting up port-forwarding on their router.

## What success looks like

- [x] This website has [documentation](/docs/node_stewards/initial_setup/coop_cloud/) with clear instructions to setup a Kiwix using Co-op Cloud
- [ ] This has been used to setup 3 nodes, each with a public URL that can be used to access the kiwix library, which includes wikipedia.

### Current status

We are working on getting test nodes set up.

- [x] [Radish house node Kiwix app](https://kiwix.radish.nodes.merri-bek.tech)
- [ ] 2nd node
- [ ] 3rd node
