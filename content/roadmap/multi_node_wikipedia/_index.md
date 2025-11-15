---
title: Multi-node wikipedia
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: A region can serve an offline mirror of wikipedia, provided with redundancy by multiple nodes
params:
  status: in-progress
---

A project to have a Wikipedia mirror running at multiple **Nodes** across a **Region** (using [Merri-bek](https://merri-bek.tech) as our test case). Things will still be changing rapidly at this point, so it's ok if these nodes are at the homes of our volunteers rather than actually at community facilities. This will still give us experience in managing redundant nodes, monitoring availability and doing traffic routing.

## What success looks like

- At least 3 nodes are running a Wikipedia mirror
- We collect basic information on the uptime of all the public urls
- Wikipedia is accessible at a local URL if on the same network as a node
- Wikipedia is accessible at a public URL specific for each node
- Wikipedia is accessible at a single public URL for the region that directs traffic to the correct region.

## Non-goals

The following things are specifically out of scope for this project, even though they might be good ideas in the future:

- Power monitoring
- Battery backup
- Solar power
- Updating Kiwix automatically
