---
title: Installing abra
date: 2026-08-03T09:00:00+10:00
draft: false
weight: 1
type: docs
menus:
  docs:
    name: Installing abra
    parent: Co-op Cloud
---

{{<hero>}}
The **Co-op Cloud** project uses a tool called **abra** (as in, abracadabra) to manage servers and apps.
{{</hero>}}

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

With that path added, running `abra --help` should work now. If it doesn't, try reaching out in the [Node Stewards chat room](https://matrix.to/#/#lores-node-stewarding:merri-bek.chat).

## Set up autocomplete

Most abra commands require typing the fully qualified domain name for your app, so we highly recommend configuring command-line auto-completion. Run `abra autocomplete -h` for more on how to do this. The instructions vary depending on which shell you use.

With autocomplete enabled, you can run a command like `abra app deploy myapp.example.com` by just typing `abra app deploy myapp` then pressing `<tab>`.

## Checklist

You should have completed:

- [x] Abra is installed on your dev computer
- [x] Abra auto-complete is setup
