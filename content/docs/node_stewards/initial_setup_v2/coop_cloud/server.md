---
title: Abra server config
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 2
type: docs
menus:
  docs:
    name: Server config v2
    parent: Co-op Cloud v2
---

{{<hero>}}
The commands on this page are all on our dev computer.
{{</hero>}}

## Creating server config

We already made our [Raspberry Pi](../../raspberry_pi) available on our local wifi, and can access it via YOUR_HOSTNAME.local. That same local address how we're going to be referring to it with `abra`.

Let's start by creating the server config:

```bash
abra server add YOUR_HOSTNAME.local
```

If that worked, you should see it in the list of servers that abra is now managing by running:

```bash
abra server list
```

And you should see this server in your list of servers.

## Creating self-signed certificates

For this server, we're going to need SSL certificates so that it can encrypt traffic. Since we're using a server at a `.local` address, we can't use the regular, internet-based ways of doing this. We need to create our own.

This uses the `openssql` tool. If your dev machine is Linux, you probably already have it installed. On other operating systems you might need to find it to install.

Here's the command. It's a pretty big one. Before you paste it into your terminal, you need to go through and replace every instance of `YOUR_HOSTNAME` with the actual hostname of your pi (eg: `lores`). You run this on your **dev machine**.

```bash
openssl req -x509 -out pi_local.crt -keyout pi_local.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=YOUR_HOSTNAME.local' -extensions EXT -config <( \
   printf "[dn]\nCN=YOUR_HOSTNAME.local\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:YOUR_HOSTNAME.local,DNS:*.local\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

When you run this, it should generate two files: `pi_local.crt` and `pi_local.key`.

If you've got those files, we're ready to move onto the next step and install something on our server.
