---
title: Installing without a publc IP address
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 1
type: docs
menus:
  docs:
    name: No IP address
    parent: Archive
---

{{<hero>}}
Accessing your node from internet is important if it's going to be a community web-host, but it is sometimes beneficial to set it up locally and get that working later.
{{</hero>}}

In the [Raspberry Pi](../../raspberry_pi) setup section, you worked with your Pi using a local hostname only, such as `lores.local`. However, when getting to the [Access from internet](../../raspberry_pi/access_from_internet) section you may have gotten stalled at getting a public IP address working, and thus a domain name.

We're [working on a better solution](/roadmap/multi_node_wikipedia/direct_local_access/) for local operation, but for now, there's a way to _"cheat"_ a bit by convincing your dev computer that your server has a full domain name when it doesn't really.

## Custom host file entry

While we generally rely on a [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) server to tell us the IP address of a computer for a particular domain name, your computer also has a place where you can set custom rules to point certain domain names at certain IP addresses. This is a plain text file, called the "[hosts file](<https://en.wikipedia.org/wiki/Hosts_(file)>)".

### Finding the local IP address of your Raspberry Pi

Your may remember from the [Logging in](../../raspberry_pi/logging_in) step, that when we run:

```bash
ping lores.local
```

Then we get back an IP address, looking something like `192.168.196.1`. Make a note of that, we'll need it in the next step.

### Editing the host file

You want to perform this step on your **dev machine**. You are telling that machine where to find your pi.

You can edit this file from a terminal, using:

```bash
sudo nano /etc/hosts
```

You can replace the word nano with the editor of your choice. These instructions should work on Linux or MacOS.

You'll see that every line in your hosts file starts with an IP address, and then has one or more hostnames, separated by spaces.

{{<aside>}}

## Still on windows?

If your dev machine is using windows, you still have a host file, it's just in a different place. Read [How to Edit the hosts File on Windows 10 or 11](https://www.howtogeek.com/784196/how-to-edit-the-hosts-file-on-windows-10-or-11/) for details.
{{</aside>}}

### Writing your dummy hostname to the hosts file

Choose a dummy hostname that you'd like to use. It can be anything, but it might be nice to use something that you actually want to use once you get internet working. For volunteers at [Merri-bek Tech](https://www.merri-bek.tech/) for example, it might be `YOURNAME.nodes.merri-bek.tech`. However, it's fine to just use `mycoolnode.com` or whatever you like.

Then, put a line in your hosts file that looks like the following:

```bash
# The following is a line to get my LoRes Node to work locally, remove it when I have setup a public IP
192.168.196.1   mycoolnode.com traefik.mycoolnode.com kiwix.mycoolnode.com lores-node.mycoolnode.com
```

In the above, replace that IP address with the IP of your pi. Also, replace that domain name (`mycoolnode.com`) with whatever dummy hostname you choose. Note that you need to specify all four versions of that. One with no subdomain, and also versions with the subdomains for the three co-op cloud apps we use in these install instructions (`traefik`, `kiwix` and `lores-node`).

If, in the future, you also add other co-op cloud apps, you'll need to add their domains to this line in your hosts file too. This is annoying, but there's no way to put wild-cards in a hosts file. If you're an advanced Linux user, you could consider using `dmasq` instead, as per [these instructions](https://stackoverflow.com/questions/20446930/how-to-put-wildcard-entry-into-etc-hosts).

### Next steps

Once this is done, you can skip over the [Access from internet](../../raspberry_pi/access_from_internet) section and start installing [Co-op Cloud](../../coop_cloud). Use your dummy hostname as the hostname of your pi when dealing with `abra`.

Don't forget though, this means that your new services on your LoRes Node are only visible when you are on the same LAN as your Raspberry Pi, and only from a computer that has that line in it's hosts file (you can put that in multiple computers). It's not intended as a way of getting our communities onto our web-servers, it's just for you to keep developing.
