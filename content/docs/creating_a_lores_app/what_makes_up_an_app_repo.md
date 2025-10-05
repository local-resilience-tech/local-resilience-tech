---
title: What makes up an App Repo?
date: 2025-09-03T09:00:00+10:00
draft: false
next: your_dev_environment
weight: 3
type: docs
---

{{< hero >}}
We need a way for people to install a **LoRes App**. Should we run one big "App Store"?
{{< /hero >}}

That's a rhetorical question. When someone _\*cough\*_ _Apple_ _\*cough\*_ controls what apps you can install, it centralises power in a way that doesn't support the sort of local resilience and grass-roots empowerment we want here.

So anyone can run an a repository of apps, which we'll call an **App Repo** here. In fact, it's not even really something you need to run, it's just a git repository.

We did consider having one git repo per app, and that would work pretty well, but some organisations might find it handy to collect all their apps in one place. You're totally welcome to have a repo with just one app in it.

## Structure of an App Repo

An app repo is just a git repository, containing a folder for each app, and no other folders (unless they are folder starting with a dot, like `.git`, which are ignored). That's it, you understand the whole thing now.

## The LoRes Apps Repo

Here's the "official" LoRes Apps Repo:
https://codeberg.org/lores/lores-apps

As you can see, it has an `example` folder that is the example app. You might want to use that as a reference.
