---
title: Regional app domains
date: 2025-10-19T09:00:00+10:00
draft: false
weight: 4
type: roadmap
summary: Some domains can be used to direct traffic to any node which has registered that app, and is known to be live
---

{{< user_story >}}
_As an_ **internet user**
_I want to be able to_ **access a web app provided by my regional group at a fixed domain**
_So that I_ **don't have to know about specific nodes**
{{< /user_story >}}

The rough idea here is that a specific domain, like `kiwix.community.net` would point to the `kiwix` app at a specific node. It could be a cname for `kiwix.library.nodes.community.net`, for example.

Additionally, as this connects into monitoring. If the node that it is pointing to is known to be down, then the node manager should automatically change it to point at a new node.
