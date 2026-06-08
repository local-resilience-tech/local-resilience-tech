---
title: Your web apps, talking peer-to-peer
date: 2026-06-07T09:00:00+10:00
draft: false
type: blog
---

If you've been following along with [Neighbourhood-first Software](/media), you'll know that it's all about using peer-to-peer (**P2P**) technologies to communicate between web servers located across your local area.

As we work through the [roadmap](/roadmap) for the **LoRes** project so far the only P2P communication between local nodes has been performed by the [LoRes Node Manager](https://github.com/local-resilience-tech/lores-node) software itself - using P2P messages for nodes to coordinate how they connect to each other in a regional network.

The goal though, is for application data to be communicated this way, so that we can have web apps running on servers around our Neighbourhood, and have them talk to each other using radio technologies (like LoRa) that work even when the power and internet are down.

## Just one P2Panda Node

LoRes uses P2Panda](https://p2panda.org/) for our P2P communication. P2Panda is a toolbox for building P2P applications that handles some of the tricky bits of managing groups of peers sharing data, storing messages and managing permissions and encryption.

Some apps have P2Panda built in, such as [Reflection](https://github.com/p2panda/reflection) (a P2P text editor for Gnome). If we did that for every web app on a server, we'd end up duplicating a lot of effort, and likely creating a lot of noise and complexity in our attempt to network these servers together.

So what we want is one P2Panda "node" on one physical server, which can be shared amongst multiple P2P aware web applications.

The LoRes Node software already runs a P2Panda node, so that can be our one node. In fact, LoRes Node doesn't do much more than that, it's features mainly relate to nodes within a region learning about each other and networking together. So not only do we have the node, we are also connected to the correct peers to share application data.

### A P2Panda API for Co-op Cloud

So how will an application use this shared node? An application essentially wants to share messages with other installations of **the same application** on other servers within the regional network. That means it needs to publish messages, and subscribe to messages with its peers.

This implies a pretty simple API, which **LoRes Node** can provide to other applications. After considering a few ways to implement it, such as a HTTP API, or using WebSockets, we settled on [gRPC](https://grpc.io/) as desired approach. Using gRPC works well, because it has a compact and efficient message format, and because it allows us to subscribe to incoming messages in a way that only hands an application the messages as fast as it can process them, and no faster, through the use of back-pressure.

Introducing, [`lores-p2panda-client`](https://crates.io/crates/lores-p2panda-client). This is the Rust implementation of a client library for our simple **P2Panda API for LoRes**.

This client allows us to publish or subscribe to message. To [publish a message](https://docs.rs/lores-p2panda-client/0.2.0/lores_p2panda_client/struct.PandaClient.html#method.publish), for example, we need to supply the following three parameters.

| param           | meaning                                                                                                                                                                                                                                                                                           |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `region_id`     | In LoRes, a region is a network of nodes. We intend it to be local (so that it can use radios and other forms of local transmission) but it doesn't have to be. Enter the region id of your node here, or if your node is connected to more than one region, pick the one you want to publish to. |
| `app_namespace` | The data that your app sends will only make sense to your app. You don't want other apps to get it, so send a string here that is unique to your app, such as the the name of your app                                                                                                            |
| `payload`       | Send your application-specific message. It can be any arbitrary bytes you want, encoded in any way that you want                                                                                                                                                                                  |

## Trying it out

How do we know if all this works? We need a simple
