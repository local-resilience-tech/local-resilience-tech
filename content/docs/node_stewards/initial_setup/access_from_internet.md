---
title: Accessing our Pi from the internet
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 7
---

{{<hero>}}
Our goal here is to host software for our neighbours and community, so we're going to need to expose our Raspberry Pi to the internet.
{{</hero>}}

This can get a little tricky to give clear instructions on, because it's different for every brand of home router, and also the type of internet connection you have can impact options here.

## Your public IP address

### Finding your IP

On the internet, each computer that can receive connections is uniquely identified by an [IP address](https://en.wikipedia.org/wiki/IP_address) (Internet Protocol address). All the computers, phones, robot vacuum cleaners and whatever else on your home network do not generally have public IP addresses (they have _local_ IP addresses on your home network so they can reach each other).

You home router though, _probably_ has an IP address. When any of your devices connects to the internet and and requests some data, that's where it's sent (and is then routed to the correct device on your local network).

There are three main options for your home, depending on how your internet service provider (ISP) works.

1. You have a **static IP address**. It's assigned to your home internet connection and wont change until you move out.
2. You have a **dynamic IP address**. It could change, perhaps if you reset your modem. Sometimes dynamic IP addresses don't change much in practice.
3. You don't have an IP address that uniquely identifies your home, instead your internet service provider uses a single address for multiple homes, then directs traffic to your house using [Network address translation (NAT)](https://en.wikipedia.org/wiki/Network_address_translation).

To find out what you have, from a device within your home network, go to an IP checker page like [iplibre.com/whats-my-ip](https://iplibre.com/whats-my-ip). You should see your IP address there.

Now that you know your public IP address, you need to find out whether it's static, dynamic, or routed via NAT. If you already know (perhaps your ISP told you), you can skip this next bit.

You can check using the `traceroute` command. On your **dev computer**, install traceroute if it isn't already. On Ubuntu Linux, for example, that's done with `sudo apt install traceroute`. Then, use it to trace the route to your public IP, again from the machine on your home network.

```bash
traceroute 199.34.228.155
```

(replace that IP address with your own public IP)

Traceroute lists all the hops needed to get from where you are, to the destination machine. For example, if you traceroute the above IP address you probably see half a dozen steps needed to get there. If you do your own, and you're not behind NAT, then you should just see one hop.

### Is your IP address suitable?

What we really want for hosting a **LoRes Node** is a **static IP address**. Many ISPs have the option to request a static IP address, sometimes for an extra charge. For now, that's what you're going to need.

We hope to support dynamic IP addresses in the future, by detecting the change and informing the other nodes in the network. It may also be possible to support ISPs that use NAT via more complex means, but that's out of scope for now.

## Your local IP address (for the Pi)

As mentioned, computers on your local network also have an IP address of their own for use within the local network, assigned to them by the router (strictly speaking, assigned to them by a system called [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) which is almost certainly built into your home router).

You can find the local IP address of your Pi from your dev computer by running:

```bash
ping lores.local
```

You should see something like:

```bash
PING lores.local (192.168.196.2) 56(84) bytes of data.
```

In this case, the IP address of local.local is `192.168.196.2`.

Since these addresses are dynamically assigned (dynamic is the D in DHCP), we don't know for sure that it won't change. It would be nice if we could ensure that it wouldn't, so that when we direct traffic to it in the next step, that will keep working.

In practice, many routers remember the devices that connect to them and give them the same address if possible. If you find that it's changing though, there are two other solutions:

### Static IP lease from your DHCP server

Your router might have a system in it's admin user interface to statically assign an IP address. On my router, I can click on a device already connected, and choose "assign static lease", and then it'll reserve that address for that device only. Even if I turn the Pi off for ages, no one else will be assigned that address and it'll be given to the Pi when it comes back.

### Static IP network configuration on the Pi

In the [ethernet](../ethernet) section, we discussed editing the netplan cloud-init file on the Pi. In this file we said yes to DHCP, with the line `dhcp4: true`. Instead, we can turn that off and specify an address manually. That does open a can of worms though, because you then also need to specify the other features you get from DHCP, such as where to find your internet gateway and nameservers.

Let's leave this as an exercise for advanced users for now. If it's something you'd like to try, you can find instructions [here](https://netplan.readthedocs.io/en/latest/using-static-ip-addresses/).

## Port forwarding on your home router

So, if we're going to have our Raspberry Pi serve traffic to the public internet, then we need to ensure that when a request comes in from the internet, our home router will direct that to the Raspberry Pi.

{{<aside>}}

### 📖 Let's talk about "ports"

The traffic that we're talking about is mostly internet traffic. The Internet Protocol uses an underlying protocol for transmitting network data called [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol). There's also a similar networking protocol called [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol) that is also used in some cases.

Both TCP and UDP send packets of data to different [ports](<https://en.wikipedia.org/wiki/Port_(computer_networking)>). Metaphorically, a port is like a socket to connect to. In actuality though, it's just a logical construct. You might send data to a computer saying _"this is TCP traffic for port 81"_ and it might decide that it is ignoring traffic on that particular port number.

There are a set of port numbers registered for different uses, and some ranges available for application developers to use for whatever they want. Here's [the full list](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers).
{{</aside>}}

To direct traffic, we're going to have to make sure our router forwards traffic on the specific ports that we want to our Raspberry Pi.

How this is done varies on each home router. In general though, your router will have an admin interface - in most cases it's a web interface that you find at the IP address of the router. The username and password are usually written on the bottom of the router. If it's _admin/admin_ now could be your moment to change it.

While logged in to the router's admin page, you'll need to find where to set port forwards. It's hard to give instructions with such big differences between routers, but you might find [this article](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router) helpful. Remember you can also ask questions on [our Node Stewards chatroom](https://matrix.to/#/#lores-node-stewards:merri-bek.chat).

You need to setup port forwards, directed at the local IP address of your Raspberry Pi, for the following port numbers:

- TCP Port `80`: For HTTP web traffic
- TCP Port `443`: For HTTPS web traffic
- TCP Port `22`: For SSH traffic
- UDP Port `2022`: For P2Panda traffic
- UDP Port `2023`: For P2Panda traffic

### Checking if it worked

One port we can check straight away is port 22 for SSH. We've been connecting so far using `ssh lores.local` which goes over the local network.

Instead, let's try using `ssh YOUR_PUBLIC_IP_ADDRESS_HERE`. You should see a message like _"This host key is known by the following other names/addresses"_ but if you choose yes, you should connect as normal. You are now logged in by routing your request via the internet. This would work even if your dev computer was somewhere else.

## A domain name

Accessing our Pi over the internet by specifying an IP address isn't ideal. We really need [domain name](https://en.wikipedia.org/wiki/Domain_name).

Now you could run out and buy a cool domain name from a domain registrar, and use that. That would be totally fine. The recommendation though is that a **LoRes Node** should be part of a **LoRes Mesh** for your neighbourhood. Given that, we recommend that you share a domain for that mesh, and use sub-domains for each node.

For example, at [Merri-bek Tech](https://www.merri-bek.tech/), a group implementing a LoRes Mesh in Naarm (Melbourne, Australia), we hand out node domains as sub-domains of `nodes.merri-bek.tech`. So if you are setting up a node at the makerspace, maybe it'd be called `makerspace.nodes.merri-bek.tech`.

If you use a system like that, probably all you need to do here is give your public IP address to someone in your group who sets up those rules (called DNS records) which specify which sub-domains go to which IP address.

{{<aside>}}

### 📖 DNS records

The [Domain Name System (DNS)](https://en.wikipedia.org/wiki/Domain_Name_System) specifies that the registered nameserver for a give domain can have records which specify the IP address to lookup for the domain or any sub-domain of it. We read domains from right-to-left, so `makerspace.nodes.merri-bek.tech` is a sub-domain of `nodes.merri-bek.tech`.

If our **LoRes Node** is accessible at `makerspace.nodes.merri-bek.tech`, then Co-op Cloud wants to use a sub-domain of that for each app that's installed. So when we install the kiwix app to display wikipedia, that will be at `kiwix.makerspace.nodes.merri-bek.tech`. Given that, we need a wildcard domain record for that too. So the records that would be needed in this example are:

```
makerspace.nodes.merri-bek.tech   A YOUR_PUBLIC_IP_ADDRESS_HERE
*.makerspace.nodes.merri-bek.tech A YOUR_PUBLIC_IP_ADDRESS_HERE
```

{{</aside>}}

### Trying it out again...

Once you've been assigned a domain name, then try that ssh test again using the new name, eg:

```bash
ssh makerspace.nodes.merri-bek.tech
```

Once again you should see the _"This host key is known by the following other names/addresses"_ message, but by selecting yes you should be able to log in.
