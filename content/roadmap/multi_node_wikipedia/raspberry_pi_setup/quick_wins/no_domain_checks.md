---
title: No domain checks
date: 2026-08-03T09:00:00+10:00
draft: false
weight: 2
type: roadmap
summary: Ensure that we don't need to disable domain checks in abra for .local domains
keywords:
  - open-to-contribution
  - good-first-issue
params:
  status:
  languages: go
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **Install apps on a .local server without learning special flags**
_So that I can_ **have my initial experience as simple as possible**
{{< /user_story >}}

## Problem

When using the `abra` application to deploy an app, with `abra app deploy`, it will attempt to connect to the server using the server's url. However, it will, by default, also perform a check that the app's url is valid.

For local network deployments, to addresses like `APPNAME-HOSTNAME.local`, this is not going to be a valid domain (according to internet based DNS) and so will always fail. These checks can be disabled by appending the `-D` or `--no-domain-checks` flag to the command. This works fine but it creates an extra thing to learn for new node stewards.

## Solution

Abra should recognise that the app domain being deployed ends in `.local` and automatically apply disable the domain checks, even if `--no-domain-checks` is not specified. It should probably output text like "Local address detected, domain checks disabled" to clarify this for the user.

## Implementation

This change will require a PR to the `abra` app.

- Abra is written in **Go**.
- The abra repository is located at [https://git.coopcloud.tech/toolshed/abra](https://git.coopcloud.tech/toolshed/abra)
- To create the PR, you'll need an account on git.coopcloud.tech, which you can get by asking in [the Coop-Cloud Tech](https://matrix.to/#/#coopcloud-tech:autonomic.zone) matrix room.
- Please do not submit AI generated PRs to abra, write it by hand or leave this task to someone else.

## Interested?

If you'd like to do this work, reach out in our [LoRes Contributing](https://matrix.to/#/#lores-contrib:merri-bek.chat) matrix room.
