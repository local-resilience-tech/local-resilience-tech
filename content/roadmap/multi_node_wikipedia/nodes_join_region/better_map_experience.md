---
title: Better map experience
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 1
slug: better_map_experience
type: roadmap
summary: Nodes can be seen on the map regardless of where they are
params:
  status: in-progress
  assigned:
    - username: exo
      role: Development
---

{{< user_story >}}
_As a_ **Community organiser**
_I want to be able to_ **see all the region nodes on a map**
_So that I can_ **start to plan how to make offline connections**
{{< /user_story >}}

At the moment, the nodes map displays a map, and it knows the lat/lng coordinates of the min and max corners of the map, allowing it to interpolate the position of any node and display a map pin.

We're deliberately not using a mapping service for this, even open street maps, so that it works simply and offline.

However, it's a shame that the map doesn't display nodes that fall outside the map, or nodes that don't have coordinates yet.

## Completed when

- [ ] If there are nodes outside the map border, the map gains a margin round the edge to display them
- [ ] Nodes outside the map bounds display in the margin area, making it clear they aren't on the map.
- [ ] An arrow to each of these nodes indicates the distance off the map
- [ ] A separate section below the map shows a simple list of nodes with no coordinates
