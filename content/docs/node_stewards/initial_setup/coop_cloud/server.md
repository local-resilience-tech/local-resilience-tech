---
title: Abra server config, stored in git
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 1
---

{{<hero>}}
The commands on this page are all on our dev computer.
{{</hero>}}

## Creating server config

We already made our [Raspberry Pi](../../raspberry_pi) available on the internet via a domain name. That public domain name is how we're going to be referring to it with `abra`.

Let's start by creating the server config:

```bash
abra server add YOUR_SERVER_DOMAIN
```

(replace `YOUR_SERVER_DOMAIN` with the correct domain for your server, eg `makerspace.nodes.merri-bek.tech`)

If that worked, you should see it in the list of servers that abra is now managing by running:

```bash
abra server list
```

And you should see this server in your list of servers, like:

```bash
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃            NAME                 ┃                HOST             ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╋━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃ makerspace.nodes.merri-bek.tech ┃ makerspace.nodes.merri-bek.tech ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

### Server config files

What did abra actually do there? It did check that our server was real, and that it could reach it (it would have returned an error if it couldn't), but it didn't actually _do_ anything to the server. Instead it just created a place for configuration files.

Where are these files? They are stored on your dev computer at `~/.abra`.

If you have there, you'll see that there is now a directory created for the server named something like `~/.abra/servers/makerspace.nodes.merri-bek.tech`. You can check this with `ls ~/.abra/servers`.

As we start adding apps to this server, configuration files will start appearing in this directory. In other words, this directory is the definition of how our server is setup. If we loose this directory, perhaps because our dev computer breaks, then we'd have to re-create our server from scratch (which would be quite tricky as it gets more complicated).

## Storing config in git

To avoid losing our server config files, and to enable us to record a version history of them, and to share them with others, we're going to put them in a [version control system](https://en.wikipedia.org/wiki/Version_control) called [git](https://en.wikipedia.org/wiki/Git).

{{<aside>}}

### 📖 Learning git

If you're writing or administering software, you will eventually encounter git. It's a version control system that was original written by Linus Torvalds to help manage the complexity of having people contribute to Linux.

Git is a great tool for managing collects of files that are mostly text in nature. That's true of our directory full of configuration files, and also true of the source code that programs are made up of.

When you use git on your machine, it provides you with the ability to go backwards and forwards through the changes you made to the files over time. However, by connecting to other machines running git we can also ensure that we have a backup offsite, and we can collaborate with others and merge in their changes.

It's not necessary to use a hosted git platform, but using one provides a lot of features that help you collaborate. For example, [here](https://codeberg.org/lores/lores-website) is the source code for this very website, on a git web app called forgejo, hosted by a non-profit community host called Codeberg.

If you're new to git, it's worth taking the time to learn it. Try [this tutorial](https://www.w3schools.com/git/).
{{</aside>}}

### Creating our local git repo

Since we want to manage that directory for our new server in git, in a terminal lets change into that directory and then initialise a new git repository.

```bash
cd ~/.abra/server/YOUR_SERVER_DOMAIN
git init
```

That should create the repository for us, but because it's empty, we can't create a first commit. Let's create an empty README file, which we can always use later to document our processes for stewarding this node. Then, we'll add this to the first commit.

```bash
touch README.md
git add .
git commit -m "Added empty README file"
```

### Pushing the repo to a public host

To gain all those collaboration and backup benefits we spoke about, we want to push our repo up to a host that supports those features.

The LoRes recommendation is that your local group could create an organisation of a git platform to support all it's Node Stewards. Self-hosting is a possibility, but for groups just starting out, perhaps the excellent [Codeberg](https://codeberg.org/) would be a good choice for this.

Within this organisation, create a repository for your new node (using the hostname as the name of the repo), and follow your platform's instructions to push your changes up there. Codeberg, for example, has instructions for _"Pushing an existing repository from the command line"_.

As an example, here's [the Codeberg organisation for Merri-bek Tech nodes](https://codeberg.org/mbt_nodes).
