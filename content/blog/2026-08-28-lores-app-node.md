---
title: A building block for Neighbourhood-First apps
date: 2026-08-28T09:00:00+10:00
draft: false
type: blog
slug: lores-app-node
author: Jade
summary: In developing the first LoRes Apps, we've found some common patterns that make development easier and packaged them up into a Rust crate called lores-app-node.
---

{{<hero>}}
In developing the first **LoRes Apps**, we've found some common patterns that make development easier and packaged them up into a Rust crate called [LoRes App Node](/contributing/software/lores-app-node/).
{{</hero>}}

## Moving beyond lores-p2panda-client

If you've been following along, and wondering how to convert a web-app, into a [Neighbourhood-First](https://tv.lumbung.space/w/nzuB248U2LQA1LCn7vYmER) web application that can sync with other local servers, you will have seen our post on the [example-chat-app](/blog/example-chat-app).

The chat app example is powered by our [lores-p2panda-client](/contributing/software/lores-p2panda-client/) Rust crate. This library allows applications to communicate with the local LoRes Node about the P2Panda messages that it wants to send to other servers in the region.

It works really well for a messaging app example. It sends messages, and it subscribes to incoming messages, giving communication between servers without the app itself needing to know how the servers are networked.

However, messaging is a very simplistic case of event-sourced apps, because messages and events are essentially the same thing.

## What is Event Sourcing?

**"Event Sourcing"** is a software architectural pattern. You can read about it in lots of places. There's a good, but fairly lengthy write up [by Martin Fowler](https://martinfowler.com/eaaDev/EventSourcing.html).

In short, event sourcing is the idea that every change to the system is driven by an event. If you have all the events, you can represent all the changes.

Event sourcing is also an example of the more general pattern [Command/Query Responsibility Segregation](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs) (or CQRS). Every time you want to **do** something (a command), you need an event. The result of the events is that things change, which is often expressed through a data representation called a "projection".

A clear example of this is a bank account. You may never have written general ledger software for a bank, but you can probably imagine that a bank doesn't just store your total balance in a database table somewhere. It could do that, but it would certainly also need to store every transaction, and it needs those to add up to the total balance. The easiest way for it to do that is to actually treat the transactions as the source of truth, and to calculate the balance from those. Thus, we would say that the projection (the balance), is _sourced_ from the events (the transactions).

## How does this apply to P2P apps?

Event sourcing is a more complex way to build software than just making changes directly to a centralised database. Like most software developers, I have been lured in by cool-sounding architectural patterns before, and I have used the event sourcing pattern to build apps that in hindsight, would have been simpler without it. So, I'd certainly advise using it with caution. However, event sourcing has a particular super power that might be relevant, it's one clear way to make apps work P2P.

Event sourced apps are well suited to P2P communication, because the current state can always be computed from the sequence of events. This sequence of events can come from a single server, or it could be all the events from all the servers, ordered in whatever way makes sense (eg. by time generated, etc).

It also makes it really clear what data each peer should send to each other. They send the events. Or more specifically, they send the events the other peers don't have yet.

This approach of sharing event logs between peers was made particularly popular by [Secure Scuttlebutt](https://www.scuttlebutt.nz/). Scuttlebutt was then the influence of many other P2P projects, including [P2Panda](https://p2panda.org/) which we use.

{{< figure src="/images/blog/2026-08-28-lores-app-node/hermes.png" alt="A stylised hermit-crab">}}

It should be said that event sourced approaches are not the _only_ way to do P2P. They work well for eventually consistent application data, but less well for synchronising a big collection of file storage.

## So can we Event Source with lores-p2panda-client?

Yes certainly, but there's a bit of work that apps will need to do themselves. As we started to build our first LoRes apps (look for one being announced in the coming weeks) we noticed recurring patterns to ensure a resilient connection and a clear connection between the events and the projection.

So, based on that work, we've released another crate (library for the Rust programming language) that does a lot of this work for you.

{{< software-link "lores-app-node" >}}

Hopefully this is useful for app development, and shows where we're going with this. If it all seems a bit abstract, stay tuned for our upcoming release where we wrap an existing open source app in order to give it Neighbourhood-first functionality by using lores-app-node.
