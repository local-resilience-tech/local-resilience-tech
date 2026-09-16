---
title: Share LoRes Node Version
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: We have awareness of the lores-node version for every node in the region
---

{{< user_story >}}
_As a_ **Region Organiser**
_I want to be able to_ **see all lores-node version of each node in the region**
_So that I can_ **help Node Stewards update to the version needed to complete this milestone**
{{< /user_story >}}

Since we're moving fast right now, there are lots of updates containing breaking changes. This will require Node Stewards who have succesfully setup their node to occasionally upgrade. One key piece of software to upgrade is LoRes Node.

## Documentation

A documentation section has been added to cover this, [upgrading lores-node](/docs/node_stewards/maintenance/software_updates/lores_node/).

## Version publication

We want a P2Panda operation that announces the current lores-node version. It shouldn't do it very often, just after each upgrade.

Probably the way to have this operation project to the database the version of each node. Then, on boot, a node can check if it's runtime version has changed relative to that projected version, if so it can send out a new operation.

If this is sent more than once, that's ok, as projection should be idempotent.

The version of each node can display in the nodes page.
