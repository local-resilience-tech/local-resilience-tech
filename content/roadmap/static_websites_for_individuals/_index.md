---
title: Static websites for individuals
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: Serving simple static websites provides our first example of P2P user configuration across the region.
---

We aim to provide value when the internet is up by hosting websites for our community. We're starting with websites individuals, with a focus on static websites only (websites that can just be served, and don't run any back-end software or databases).

This provides value to a community by:

- Encouraging websites rather than profiles on "tech platforms" to help people escape lock-in
- Reducing carbon emissions and e-waste. Static websites take _way_ less resources to host
- Encouraging tinkering and understanding how the web works

As part of hosting websites, we can also develop a view of how we can provide redundancy between different Neighbourhood-First nodes, and what sort of availability is appropriate and achievable in a community context.

Additionally, as this milestone builds on [Multi-node Wikipedia](../multi_node_wikipedia), it requires us to develop a system for user accounts and login, users adding or requesting website hosting, and also backup of the website data.

## What success looks like

- [ ] Individuals can have accounts with the region, replicated across all nodes
- [ ] Individuals with an account can create one or more websites
- [ ] Node Stewards can configure an overall limit on website file storage, as well as a limit per website (and/or perhaps per user)
- [ ] Websites have files which can be uploaded by users
- [ ] Websites can have a domain name, which can be a subdomain of one managed by the region
- [ ] Websites are synchronised across multiple LoRes Nodes
- [ ] Websites files can be restored in the case of a total node failure
- [ ] Websites are available on the local network that a node on, even if the internet is down
- [ ] Websites are available on the internet, served at a url pointing to a node thought to be up
- [ ] We provide rough monitoring and availability reporting for websites

## Non-goals

The following things are specifically out of scope for this project, even though they might be good ideas in the future:

- Websites for organisations (to avoid the complexity of group management and permissions)
- Content management websites, static site generators, etc
