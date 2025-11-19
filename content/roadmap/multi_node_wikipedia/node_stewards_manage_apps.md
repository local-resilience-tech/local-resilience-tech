---
title: Node stewards manage apps
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: Node stewards can install and upgrade apps from any repository
params:
  status: done
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **install, upgrade and remove apps**
_So that I can_ **leverage open source software to provide features to my local community**
{{< /user_story >}}

A **LoRes Node** is essentially useless without apps. Apps deliver all user facing functionality. We wanted to use a recognised standard for apps so that they can also be managed and debugged with other tools. Given that, we have decided that nodes run Docker swarm, and an app is a Docker stack. Details and rationaille for that are in our documentation [here](/docs/creating_a_lores_app/what_makes_up_a_lores_app/).

So, while docker stacks can be managed using command line tools, or graphical tools like Portainer or Swarmpit, a **LoRes App** also needs a concept of versioning, and being installed from some repository that defines multiple version. This concept doesn't exist in docker, and so we will create it. Lores Node will provide a management interface for installing apps from repostories. Once apps are installed, tools like Portainer will work to inspect what is going on and help perform more detailed administration and debugging if needed.

## What success looks like

- [x] Lores App repositories are simple and can be run by anyone (suggest we just use git repostories).
- [x] Node Stewards can install a repository and see the apps, and the versions of those apps, that it contains.
- [x] Node Stewards can install an app
- [x] Node Stewards are made aware if there is a newer version of an app available
- [x] Node Stewards can upgrade an app
- [x] Node Stewards can start or stop the app in docker swarm
- [x] Node Stewards can delete an app
