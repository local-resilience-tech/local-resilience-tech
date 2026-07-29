---
title: Abra server config, stored in git
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
