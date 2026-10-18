---
title: Offline websites with Kiwix, but P2P
date: 2026-10-18T09:00:00+10:00
draft: false
type: blog
slug: kiwix-p2p
author: Jade
summary: Xx
---

## Kiwix

If you haven't used it, the [Kiwix](https://kiwix.org/) project is a wonderful way to ensure that important global knowledge is available even when the internet is not.

Kiwix is rn by a non profit org, which produces regular archived copies of important websites like Wikipedia, Project Gutenberg, and iFixit. You can browse their full set of libraries, in a range of languages, at [library.kiwix.org](https://browse.library.kiwix.org).

{{< figure src="kiwix_library.png" alt="A screenshot from the official kiwix library showing search filters, 1312 books present, and a grid of the top six books" caption="The kiwix library at library.kiwix.org">}}

You can already install Kiwix with Co-op Cloud, in fact we maintain [the recipe](https://recipes.coopcloud.tech/kiwix). Installing that on your LoRes Node would already provide some great information to support local resilience.

## From Offline to Connected

The **LoRes Mesh** is a project to connect existing open source web apps, with a resilient local peer-to-peer (P2P) network. So what could a library of website archives possibly need with P2P?

Well, what do physical libraries use connectivity for?

Where I live, if I got to my local library to borrow a book, their computer systems can also be used to find out if the book is at another library nearby, allowing me to go there, or request that it be sent to my library.

**Kiwix on LoRes works the same way.** Each Kiwix install knows about the archives on each other node in the region.

{{< figure src="lores_kiwix.png" alt="A screenshot from the lores kiwix, showing 4 books. Two of them look like the normal kiwix book tiles. Two more have paler backgrounds and dashed borders, indicating that they are only available remotely" caption="LoRes Kiwix, showing two books from other nodes">}}

In the above example, this server has Kiwix archives for Wikipedia and iFixit, but it know about two other archives (PhET and Wikivoyage) that are located on other servers.

## How does this work?

In the article, [A building block for Neighbgourhood-First apps](/blog/lores-app-node/) we talked about how apps installed on a **LoRes Node** could send events to other instances of the same app in the region, letting the node handle all the complex P2P stuff. You may also recall we wrote about how [Regions are local digital commons](/blog/binding-apps-to-regions/), networks representing a geographic area big enough to be useful, but small enough to do community governance.

So, to connect Kiwix to this LoRes world, we've released `lores-kiwix`.

{{< software-link "lores-kiwix" >}}

## LoRes Kiwix

Wrapping Kiwix in a Rust app is a good example of how we can connect existing open-source software to our P2P network without forking it.

Kiwix is deliberately written as a stable C++ app, shipped on linux systems as `libkiwix`. That libkiwix package includes `kiwix-serve`, which serves up the web interface.

The lores-kiwix wrapper needs to intercept API calls for the search page, to append the remote books to the end of the results. It also intercepts API calls to build the filters, so remote books will have their languages and categories added to the search options.

Finally, when the wrapper boots, it uses lores-app-node to send events if any books have been added or removed, keeping all other nodes in the region up to date with everyone's catalogue.

This creates some maintenance burden, but the most important parts of Kiwix (serving up the detailed zim files) is left untouched. Upgrading this wrapper to use newer versions of kiwix should be very straightforward.

## Ready to use

You can use this now. We have full install and setup instructions available in the [Kiwix Library documentation for Node Stewards](/docs/node_stewards/kiwix_library/).

Most importantly, this example paves the way for other LoRes apps to use similar techniques.

With lores-kiwix released, we expect to be able to rapidly moved through the remaining cleanup for our first major milestone in our [Project Roadmap](/roadmap/), _"Multi-node wikipedia"_.
