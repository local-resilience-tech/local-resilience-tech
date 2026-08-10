---
title: Remote Kiwix Info
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 9
type: roadmap
summary: The kiwix app displays which nodes have each zim
---

{{< user_story >}}
_As a_ **User**
_I want to be able to_ **see all the zim archives available in my region**
_So that I can_ **go the right location in an outage to get the info I need**
{{< /user_story >}}

The Kiwix user interface normally displays a search-able index of the archived websites (stored as `.zim` files) available on this web-server.

For our LoRes version, we want to also display zims that are not actually on this web-server, but are on other nodes in the region. We want to do this in a way that doesn't confuse the user into thinking they can access them, but we do want to show them where they can be found. Kind of like when you search for a book in a library and find that it isn't there, but it is available at another local library.

## Feature ideas

- [ ] Zim files that aren't present display in the list, but with dashed borders and less bold colours
- [ ] These remote zim files also show which nodes are hosting them
- [ ] There's a filter option to show "locally available only" zim files
- [ ] If the internet is up, you can actually link directly through to the remote zim file on the other server

## Implementation

### Sharing the zim index

When a kiwix install detects that a zim has been added, it needs to share that with the region (using an app specific p2panda message). That's how we build up a view of what is available.

### The user interface

We may want the user interface to look almost exactly like the default kiwix interface, with our additions. So it doesn't look like we're usurping the project.

### Wrapping libkiwix

Kiwix is written in C++. We are running the `kiwix-serve` app, but actually this is just a thin daemon wrapper around [libkiwix](github.com/kiwix/libkiwix). It's libkiwix that produces the front end, using a C++ powered web server, some static html, and JS.

We could run this entire website, wrap it, and inject our extra stuff with some javascript. However we also need to monitor when zims are added. Also, libkiwix is designed to be used as a lib too, and built upon.

So the likely implementation path here is to implement our own web app, that uses libkiwix, and we decorate it as we like (and respond to library changes).

Ideally, we'd write this in Rust. To access libkiwix, there don't seem to be any existing rust bindings. We may want to do this using the [cxx](https://lib.rs/crates/cxx) crate.
