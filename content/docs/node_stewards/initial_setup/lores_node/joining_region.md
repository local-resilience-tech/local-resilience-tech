---
title: Joining a Region
date: 2026-03-08T09:00:00+10:00
draft: false
type: docs
weight: 4
menus:
  docs:
    name: Joining a region
    parent: LoRes Node
---

{{<hero>}}
We're only going to change the world if we work together. A **Region** in **LoRes Mesh** is made up of nodes, connected via peer-to-peer communication, working together as a digital commons.
{{</hero>}}

## Creating a region

For the purpose of these instructions, someone in our group has presumably created the Region already. You can take a look at the "Create New Region" tab to see what's involved in this.

Given that someone has done this already, they can share the region ID with us, which is used to join. The region ID isn't intended to be secure, but keeping it private could reduce unnecessary join requests. We suggest that groups don't put it on their website or anything, but pass it on directly to people setting up new nodes.

## Joining a region

So assuming that we've got a region id, can click the "Setup region" link in the sidebar to see the form below.

{{< figure src="/images/node_stewards/lores_node/join-region.png" alt="A screenshot with the text: Join a Region. A Region is a cluster of Nodes that are in regular communication, and provide services to users that are redundantly available at many or all of the Nodes. This means that a region is generally a geographic area that makes sense to humans, such as your neighbourhood, town, river catchment, etc. You can either join an existing region, or create a new one. What would you like to do?">}}

Filling this out is essentially just information provided as part of the join request. It's not secure, so stick to public information about your request, or if you'd like a more provide conversation about it, indicate that you'll reach out via another channel.

Once you submit this request, you will be subscribed to information about the region, but your application will be pending.

## Getting approval

Right now, approval is centralised. Whichever node created the region needs to approve your request. That's not what we envisage in the future, as something more suited to decentralised systems and commons governance would imply not putting a single node in charge of who joins. We recommend that groups still use democratic group processes to determine who joins, and just use the approving node to carry out the group's wishes.

However, in order to get your join request approved, your message is going to need to be able to reach the region creator node, and their response is going to need to reach you. This relies on successfully passing messages between peer nodes in the region, a process that's called **Gossiping**, and is covered on the next page.

## Checklist

You should have completed:

- [x] Sent off a request to join a region.
