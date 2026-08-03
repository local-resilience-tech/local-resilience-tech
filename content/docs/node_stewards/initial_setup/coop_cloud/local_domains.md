---
title: Local domains for apps
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 3
type: docs
menus:
  docs:
    name: Local domains
    parent: Co-op Cloud
---

{{<hero>}}
When we're hosting web apps with Co-op Cloud, each app needs it's own unique domain. Since we want this to work offline on our local network, there are a few bits of magic needed.
{{</hero>}}

On the internet, if you register a domain like `mynode.org.au` then you also have the ability to use any "sub-domains" that you want, like `app1.mynode.org.au`.

{{<aside>}}

### Domains are read from right to left

If you read the parts of a domain (separated by periods `.`) from right to left, they go from the general to the specific. In the example above:

- `au` is for any computer in Australia
- `org.au` is for charities and non-profit organisations (in Australia)
- `mynode.org.au` is something that a specific organisation can register
- `app1.mynode.org.au` is something even more specific, what it means is up to us (such as an app on our node)

And there is no real limit. You could use `feature1.app1.node1.organisation1.bioregion1.lores.org.au` if you want, but people may not love typing it in. Also note that while as technologists we may learn that this is how domains work, not all our users understand this and we can confuse them.
{{</aside>}}

## Local network domains

The domain system is really intended to be used for computers connected to the internet, however there are a couple of options available in an offline situation.

- `localhost` always refers to the computer you are using
- `.local` refers to computers on your local network

This second example, `.local` is what we're going to use here. When we worked on [Logging in](../../raspberry_pi/logging_in) to our raspberry pi, we learned that if we gave it a hostname like `lores`, then we could log into it at `lores.local`, which is powered by [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS).

{{<aside>}}

### mDNS doesn't have subdomains

We still read these local domains from right to left, so `lores.local` means _"the computer called `lores` with the `local` network._

So, does that mean we can have a subdomain of that like `email.lores.local`? Unfortunately not really 😭. The mDNS protocol was only really envisaged as a simple way to find the printer on the network. It's actually possible to get your computer to say that it wants traffic from `email.lores.local`, but that wont work on a lot of computers, particularly those running windows.

{{</aside>}}

Given all this, we need a local domain for each app, and we can't use subdomains, so at **LoRes** we've decided to use hyphens. Our apps will be at local domains like `email-lores.local`, or more generally `APPNAME-HOSTNAME.local`.

## Installing Co-op Cloud mDNS Publisher

To cut a long story short, there's just one thing we need to install on the Raspberry Pi in order to make it announce a local domain for each app, it's a little app we built called `ccmdns`.

You download it by running the following commands on your Raspberry Pi.

```bash
wget https://github.com/local-resilience-tech/coop-cloud-mdns-publisher/releases/download/v0.3.0/ccmdns_0.3.0-1_arm64.deb
sudo apt install ccmdns_0.3.0-1_arm64.deb
```

Once it's installed, it stays running, and for every Co-op cloud app you install, it will automatically start publishing a new domain on your local network of the form `APPNAME-HOSTNAME.local`.

## Checklist

You should have completed:

- [x] ccmdns is installed and running on our Raspberry Pi
