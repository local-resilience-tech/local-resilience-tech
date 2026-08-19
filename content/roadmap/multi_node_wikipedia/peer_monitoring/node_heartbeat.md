---
title: Node Heartbeat
date: 2026-08-18T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: Node's monitor a heartbeat message from each other, and report to stewards which nodes are up on the P2P network
params:
  status: in-progress
  assigned:
    - username: Greg
      role: Development
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to_ **know how recently we were able to exchange messages with each node in the region**
_So that I can_ **troubleshoot any connection problems**
{{< /user_story >}}

In lores-node, when viewing the list of nodes for a region at `/regions/jade-test/nodes?tab=list` we want to be able to see how recently each node was verified to be in communication with us.

While we may be able to directly ping _some_ nodes, with most nodes in our region we will only ever exchange messages via an intermidary. Thus, this task focusses on measuring that message exchange, not direct communication.

## Visible result

It would be good if each node in the list displayed some information about when they could last communicate with the node we're looking at. The Node Steward is usually going to be using this to just "was it recent?". So, it is recommended that this display is relative. Perhaps a simple "green light" indicator for when it was "recent enough". If it has gotten too old, then some text indicated "last reached 2 hours ago" might be good.

## Implementation

### Frontend

I'd recommend that the backend tells the frontend the "last heartbeat at" time, and the frontend does all the relative time display. This should not require a refresh.

### API

This may need changes to the information that we load from a node, and also a new websocket message on each heartbeat from each node. That's a fair few websocket messages. I'd start with leaving that always on initially, but if it's too much, we could consider only doing it when there is a user looking at the page that displays that data. That would require API messages to the server telling the server when to start and stop subscribing to socket notifications for the heartbeats.

## P2P

Essentially, each node needs to publish a heartbeat message on a regular cadence, perhaps every 5 minutes for now. This will go to all other nodes. We don't want the nodes to write it to their P2Panda log though, although that will grow too much. To avoid this, P2Panda has a concept of [Ephemeral messages](https://docs.rs/p2panda/latest/p2panda/struct.Node.html#method.ephemeral_stream), which are only sent to nodes who can currently receive them, and are not logged. You can see a good example of these used for heartbeats in the [Reflection](https://github.com/p2panda/reflection) text editor.

Although we don't log the message, we do want to do something with it when we receive it. We could persist it to our projection database, writing a most recent heartbeat time to each node each time we received one. This would cause one database write per node every five minutes. Maybe we don't need that yet, and instead we can just send it to the frontend websocket only.

That will mean that when you first view the nodes page, every node will be old (or maybe in an "unknown") state, and then if you wait up to five minutes, it will flesh out.
