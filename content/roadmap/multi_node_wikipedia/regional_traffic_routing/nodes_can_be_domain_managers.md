---
title: Nodes can be domain managers
date: 2025-10-19T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: A LoRes Node may be a manager node for a given domain name, providing the ability to use it to the rest of the region.
params:
  status: in-progress
  assigned:
    - username: gerg
      role: Development
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **connect to a DNS service API**
_So that I can_ **make a specific domain or domains available myself, and other nodes in the region**
{{< /user_story >}}

## Domain management

Domain management is an optional feature of a node, not all nodes will use it. Domain management is a feature of the node, not the region, but it makes specific domains available to specific regions.

## Accounts

If domain management is used by a node, it may add one or more "Accounts". These are API accounts for DNS providers. We will presumably need to build these per provider. Our goal is not to use the hyper-scalers here, especially Amazon, so this might require multiple integrations with small providers being used by local groups. As our launch groups are in Naarm, we would like to start by implementing an API client for Serversaurus.

Node stewards add accounts to the node, for each account they pick an API (eg, serversaurus) and enter credentials. For an account, they may choose to manage all domains, or specific domains only (the latter being an imperfect safety net when we can't create an API account for the node limited to only the domains we want it to change).

## Domains

The domain management screen shows all the domains that this node is managing, across all its accounts. Each of these represents
a publicly registered domain (the sort of domain you buy from a registrar, one level below a TLD) that thus has a zone file to manage it's DNS records.

For each domain, the node stewards can list a number of **available domains**, which may be the domain itself, or any of it's sub-domains (any level deep). Available domains are made available to a specific **Region** (see below).

Node stewards can also list any number of **restricted domains** which are not available, overriding available domain rules. The later allows you to protect important subdomains (eg, `mail.community.net`) while still allowing other subdomains (`*.community.net`).

## Available Domains

Available domains are specific domains, or wildcard domains, made available to a specific Region.

Available domains may start with a wildcard, in which case you are making available specific slots in the wildcard. For example, if a node manages `community.net` and it decides to make available `*.nodes.community.net`, the it is saying that nodes in the region may ask to claim specific subdomains. For example the library could claim `library.nodes.community.net` while the makerspace claims `makerspace.nodes.community.net`. If instead the domain is made available without the `*` wildcard symbol, eg `nodes.community.net` then only one node can claim it, and no others can. For this reason it is recommended that we display a warning when listing an available domain that is not a wildcard.

Eventually it is likely that available domains will be announced via P2Panda messages to the region, but that isn't needed as yet.

## Claiming / Assigning Domains

Available domains can be claimed by nodes. While it is envisaged that nodes will be able to request domains, it should also be possible for the Node Stewards of a node that is managing a domain to assign them directly.

As such, for each available domain, there should be a feature to assign it (or more commonly a slot in it's wildcard). Using the example above, a Node Steward could thus assign `townhall.community.net` to a specific node, by entering the **node id** of that node.

## Getting IP address from node id

Nodes may or may not have a public IP address, depending on whether they have been setup to serve web traffic to the public internet (generally by port-forwarding on the router). If they do, then they need to announce it. If it has been announced, then other nodes in the region (including any managing domains) will know about it. It's up to nodes to announce if their IP address has changed.

## Updating DNS

When a domain or subdomain has been assigned (or claimed) to a node, and we have an IP address for that node, then we are ready to publish or change a DNS record using the API for the provider we are using for that domain.

Nodes managing domains should not spam the DNS registrar, as such they should build a list of pending changes they want to make, then write them with throttling. Depending on the provider, this may be an API call per domain, or it may be be that batched calls can be made. Recommended throttling is one API call every 10 seconds, no need to be fast here.

Updating DNS should be done with some awareness of race conditions. We don't know if this node is the only thing editing this domain. It ideally should not, for example, read the entire zone file, then post a new entire zone file, as perhaps other editors might be making changes (such as human admins editing the mailserver record).

A log of all DNS updates and their timestamps should be kept, and be viewable in the interface by node stewards.
