---
title: Local first setup
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: Setting up a node starts on the LAN and can be tested locally
params:
  status: in-progress
  assigned:
    - username: jade
      role: Development
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **setup my pi and see it working without having to configure my router, get an IP or a domain name**
_So that we can_ **avoid obstacles to learning and success**
{{< /user_story >}}

This requires a landing page that works over mDNS.

## Key Risk

Can we use HTTPS for a domain like `lores.local`. If not, will common browsers accept an HTTP connection instead.
