---
title: Configurable by Node Stewards
date: 2025-09-17T09:00:00+10:00
draft: false
weight: 2
slug: configurable_by_node_stewards
type: roadmap
summary: Apps defined configuration is presented to node stewards, for example a kiwix app might choose which libraries to serve.
params:
  status: testing
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **understand what configurable options my apps have, and set values for them**
_So that we can_ **build apps that have configurable capabilities**
{{< /user_story >}}

We envisage that apps might want to present configuration to node stewards. This will need apps to effectively have a **Configuration Schema** of some sort - an opinionated stance on what configuration questions there are, and what acceptible values for them are.

The specific values for the configuration need to be stored somewhere on the node, such as in a file or a database.

Since we need a way to validate the configuration values against the schema, a way to explain to the Node Stewards what the options are, and a way to log when node stewards make changes (and which ones), it seems better to provide a UI in **Lores Node** for managing configuration.

## What success looks like

- Apps can specify a configuration schema
- LoRes Node presents a UI to Node Stewards for setting configuration that is compliant with the schema
- Configuration is stored on the node and will be accessible even if the node consists of multiple machines
- Configuration is available to the app in a useable way when it is running
- Configuration can be changed and those changes will be applied to an installed app. If this requires restarting the app, that will be made clear to the user

## Non-goals

- Upgrading configuration. At this stage lets just assume that configuration is forward compatible.

## Tasks Status

- [x] If an app has a configuration schema file, display configuration option
- [x] For configuration schema, display form
- [x] Save form values to a config file for the app
- [x] App can access the config file on a shared mount
