---
title: "Part 2: Wikipedia managed with Co-op cloud"
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 2
---

{{<hero>}}
We have a [Raspberry Pi](../raspberry_pi) ready to be our server, in this section we'll use **Co-op Cloud** to install the **Kiwix** app, which will serve up our copy of Wikipedia.
{{</hero>}}

Co-op Cloud consists of a set of [recipes](https://recipes.coopcloud.tech/) for different apps, and a command line tool called [abra](https://docs.coopcloud.tech/abra/) which is used to configure and deploy them to servers.

Our instructions here follow a similar path to the Co-op Cloud [New operators tutorial](https://docs.coopcloud.tech/operators/tutorial/), but with more opinionated choices made for our setup.

## Installing abra

You should install abra on your **dev computer** by running this script (want to check it for nasties? here's [the source](https://git.coopcloud.tech/toolshed/abra/src/branch/main/scripts/installer/installer))

```bash
curl https://install.abra.coopcloud.tech | bash
```

The installer will verify the downloaded binary checksum. To validate that everything is working try running it with the help command:

```bash
abra --help
```

If you get a _"command not found"_ error, it may be because the installer placed the executable in the `~/.local/bin/` directory, but this isn't in your `$PATH`.

To add this to your path temporarily, you can run `export PATH=$PATH:$HOME/.local/bin`, but that will need to be done each time you login. A better approach is to add this command into the configuration file for your shell. There's more than one shell available for Linux, so it might be worth consulting [these detailed instructions](https://itsfoss.gitlab.io/post/how-to-add-a-directory-to-path-in-linux/#method-2-permanently-adding-to-path).

With that path added, running `abra --help` should work now. If it doesn't, try reaching out in the [Node Stewards chat room](https://matrix.to/#/#lores-node-stewards:merri-bek.chat).

## Set up autocomplete

Most abra commands require typing the fully qualified domain name for your app, so we highly recommend configuring command-line auto-completion. Run `abra autocomplete -h` for more on how to do this. The instructions vary depending on which shell you use.

With autocomplete enabled, you can run a command like `abra app deploy myapp.example.com` by just typing `abra app deploy myapp` then pressing `<tab>`.

## Lets install our apps

In these sections we'll be using abra to install and configure our apps, and we'll also make sure that we've got that configuration stored safely where we can share it with other Node Stewards.

<!-- {{<todo>}}
Layout a plan for using abra to install traefik and kiwix. Include how a Node Steward will store and manage the config files. The steps we need to cover, in order, are:

- Installing abra in your dev computer
- Adding the host for your Pi
- Putting your config files in git to share with other Node Stewards
- Installing traefik
- Installing kiwix
- Downloading the zim file you want and plugging it into the Pi
  {{</todo>}} -->
