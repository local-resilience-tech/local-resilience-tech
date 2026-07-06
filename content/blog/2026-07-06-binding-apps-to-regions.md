---
title: Binding apps to Regions in a LoRes Mesh
date: 2026-07-06T09:00:00+10:00
draft: false
type: blog
slug: binding-apps-to-regions
author: Jade
summary: We've made a breaking change to the API that apps use to talk to lores-node, in order to better support each LoRes Node being part of multiple Regions. This change is a bit philosophical, so lets talk first about what a Region is, and why it supports good stewardship of the commons.
---

{{<hero>}}
We've made a breaking change to the API that apps use to talk to lores-node, in order to better support each LoRes Node being part of multiple Regions. This requirement is a bit philosophical, so lets talk first about what a Region is, and why it supports good stewardship of the commons.
{{</hero>}}

## LoRes regions

### A "region" in a LoRes Mesh is a commons

**LoRes Mesh** is designed to be _public interest infrastructure_. We're trying to sit in the grey area between a network of autonomous servers and a centralised system. Instead, we're looking for multiple parties to come together and collectively steward a **commons** that is important for everyone.

To do that, it sometimes helps to have a bounded area of the mesh where we have a better idea of who needs to work together, and what other stakeholders might need to be involved or considered. This bounded area we use in LoRes is called a "**Region**".

We could have easily called it a "network", but we wanted to hint that it would often be geographical in nature. You can use the `lores-node` app to manage a "Region" of servers selected in any way you like, it doesn't have to be geography - but if you don't consider geography then you might struggle to use resilient communication technologies like LoRa to connect your servers.

A Region can be any size that you like. The main things that dictate it are not technical, but instead based on what sized group of people you would like to see working together. I would suggest that one consideration for this might be that it should be possible to engage in some form of democracy with the stewards of all the nodes in the Region, whether that's an online process, or an in-person assembly.

### Nodes are autonomously managed collaborators within that commons

Each **LoRes Node** is a server that provides digital services (web applications mostly) to people around it. Even one node is useful, but it's by connecting them together that we form a LoRes Mesh. Nodes within a Region are not centrally managed, but they (or rather their human **Node Stewards**) are collaborators in the ongoing process of [commoning](https://commonslibrary.org/practising-commoning/) in that Region.

### Leaving room for future ecosystem visions

We're focused just on Nodes and Regions right now, but it's important to leave room for networking beyond this local scale. In the future we might imagine:

- **Multiple regions might network together** into a larger aggregate commons that is stewarded through delegates from each region. Perhaps there are more than two levels of this, supporting bio-regions, continents, and the whole planet.
- **Aggregations of regions might be plural**, rather than a single hierarchy, representing different ways of grouping technology, people and planet. For example, along watersheds, political boundaries, and first-nations concepts of country.
- **Agreements might exist between two neighbouring Regions**, independent of a larger commons, to exchange data, storage, power or otherwise work together.
- **Some Nodes might be mobile**, mounted in vehicles or backpacks and exchanging data either within a Region, or amongst Regions, acting as a [sneakernet](https://en.wikipedia.org/wiki/Sneakernet) at the server-to-server scale.

We don't know yet which of these ideas will emerge, so it's important that the technical implementation leaves room for many possibilities, and we have done so in a number of ways.

One way, that we'll be talking about today, is that `lores-node` does not assume that a Node belongs to one-and-only-one Region.

## How lores-node manages regions

### New Nodes might often be without a Region

As a Node Steward setting up a new LoRes Node, it seems reasonable that I might do this alone. LoRes Mesh is designed to be grass-roots infrastructure. No committee needs to assign me the task of setting up a Node, I can just decide to do it - providing immediate useful digital services for your household, community centre or [Lifehouse](https://www.versobooks.com/en-gb/products/2536-lifehouse).

### Nodes might join a sandbox Region before a real one

Depending on how a regional commons is managed, there might be some process required for a Node to join the Region. At a minimum this might be agreeing to a code of conduct and decision making process, or there might be a more relational process that allows the Region to grow "at the speed of trust".

Given these barriers, new Node Stewards might want to practice the technical aspects of networking their Node into a Region by joining a "test" or "sandbox" region, allowing them to safely experiment with peer-to-peer connections with other Nodes. At some point, they might then be ready to join a "production" Region with real users, and will need to move across. Possibly this could be a case of delete the Node and start again from scratch, but it'd be much better if a Node setup could be tested in a sandbox before being connected to live users.

This necessitates both leaving a Region and joining a new one, and perhaps could be smoother if the services on the Node could be moved over one-by-one, during which period the Node is in _both_ regions.

### Given this, a Node can be in multiple Regions

For all the reasons above, `lores-node` supports the Node being in more that one Region. In the interface you can join your first Region, and basically ignore this feature, but you can always join another Region, and then switch between them in the UI.

{{< figure src="/images/blog/2026-07-06-binding-apps-to-regions/select-region.png" alt="A screenshot from a web application the navigation sidebar. The region heading has a region name active, Third-fish Test, and shows a drop down dialog with another region, test 2, and an option to join another region.">}}

## Connecting apps to regions

So now we come to how it actually works with the software. In the blog post detailing [the example chat app and the gRPC API](/blog/example-chat-app/), we indicated that a chat app would specify a Region when posting or subscribing to messages, and indeed that how [v0.2.0 of the lores-p2panda-client](https://crates.io/crates/lores-p2panda-client/0.2.0) worked.

That wasn't a great solution, because it's not up to app users to decide to switch Regions, and as we started to build apps more complex than just chat, we noticed that switching Regions is a really big deal. The app's data is built up from the P2P messages exchanged across the Region, and so it may need a fresh database every time it switches.

### Binding apps to regions in lores-node

Selection of which Region an app is using is really an issue for the Node Stewards, and so we've now built it into lores-node. Now, when logged in as a Node Steward, you can register an application with the currently selected Region.

{{< figure src="/images/blog/2026-07-06-binding-apps-to-regions/app-without-region.png" alt="A screenshot showing the detail page for the chat-example app. It contains some details about the app, and a primary action button saying: register with thirdfish-test.">}}

Registering with a Region means that this Node records that this application instance is bound to that Region, and so all messages sent and received on it's gRPC API will be shared across the [P2Panda](https://p2panda.org/) topic for that Region.

It also announces the presence of the app to other Nodes in the Region, so that we can build up a map of which software is hosted on each Node.

### App instances

As part of this, apps now send an `instance_id` along with their app name. This can be any string, it just needs to be unique for this app on this node. This can be used to run an app multiple times (perhaps for different regions), but it can also be used to differentiate an old installation of an app to a new one. An application can get it's instance id however it likes, but the obvious method is to get it from an environment variable, configured in the Coop Cloud install config.

## This is a breaking change

This is a breaking change to the [lores-p2panda-client](https://crates.io/crates/lores-p2panda-client) rust crate, now at v0.3.0. Regions are out, app names and instance ids are in. The [command line example](https://github.com/local-resilience-tech/lores-node/tree/main/backend/lores-example-app-cli) and the [lores-chat-example](https://github.com/local-resilience-tech/lores-chat-example) have been updated to use this version. Make sure that you are running at least v0.20.1 of `lores-node`, and you should be good to go.
