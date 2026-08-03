---
title: Gossiping with peer Nodes
date: 2026-03-08T09:00:00+10:00
draft: false
type: docs
weight: 5
menus:
  docs:
    name: Gossiping
    parent: LoRes Node
---

{{<hero>}}
**LoRes Nodes** need to be able to coordinate with each other over a range of technologies, some of them quite slow and intermittent, like radio transmissions. To do this, we don't assume that the entire network is always connected, we pass messages on from node to node around the Region. This approach is called a [gossip protocol](https://en.wikipedia.org/wiki/Gossip_protocol).
{{</hero>}}

## Regional gossip

Each region has it's own "topic", within which the nodes pass on messages to each other until every node has every message ([eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency)). To do this, a **LoRes Node** doesn't need to connect to every other node in the region, just to some of them, as messages will be relayed.

But how does it find at least one other peer to connect with?

## Peer discovery

The peer-to-peer code in lores-node has two main methods of discovering other nodes. The first, is on the local network. If you're setting up your node on the same wifi as another node in the same region, they should find each other successfully. You can then take one of the nodes to another network (connected to the internet) and they will still be able to find each other.

The second method is topic-based discovery. Providing a node knows about at least one other node who also subscribes to the same topic (ie, is in the same region), then it will automatically ask that node about it's peers, and explore the network that way.

As you can imagine, this combination doesn't work if we're setting up a node on a network by itself, and also our node doesn't know about any other nodes to get started with topic-based discovery.

## Bootstrap nodes

We solve this problem by specifying bootstrap nodes. Nodes are identified by an ID, which is a 64 character hexadecimal strong. If we know the ID of one other node in the region, then we can find it and start the process of connecting. This approach is well suited to wiring things up even when internet connectivity is poor.

In the sidebar, select P2Panda node, and you'll have the ability to add another node's ID as a bootstrap node. You can also see your node ID on this page, so you can pass it to someone else if they need to do the same.

{{< figure src="/images/node_stewards/lores_node/add-bootstrap-node.png" alt="A screenshot with the heading: This P2Panda Node. Also, a button titled Add bootstrap node has been used to open a form taking a Bootstrap Node ID.">}}

## Checklist

You should have completed:

- [x] Added the ID of another node in our region as a bootrap node
- [x] Had our join request for the region approved
