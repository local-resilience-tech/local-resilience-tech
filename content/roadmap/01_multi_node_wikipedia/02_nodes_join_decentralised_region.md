---
title: Nodes join decentralised region
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: Nodes can join a region without relying on their being a centralised master node
params:
  status: testing
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **connect my node to a Region**
_So that I can_ **join other nodes to provide a shared service**
{{< /user_story >}}

It's important that nodes are able to connect to a region in a decentralised way. There shouldn't be any single point of failure for this. We've using a peer-to-peer technology for communications like this, so to connect into a region, you only need to know the details of one other node.

As a first pass, there's no consensus or authentication needed for this. You say that you're in a region and you're in it.

That's not going to work in the longer term, as bad actors will just join a region. For a more detailed approach, we're waiting for the next release of the P2Panda library, scheduled sometime in October, as it will implement group access control as outlined in [this blog post](https://p2panda.org/2025/07/28/access-control.html).

## To reach testing stage we need

- Nodes can "create" a new region
- Nodes can specify a region, and another node, and join that region

## To be production ready we need

- Nodes can request to join a region
- Some sort of consensus approach to allowing a node is needed (a certain % of existing nodes needs to agree perhaps)
