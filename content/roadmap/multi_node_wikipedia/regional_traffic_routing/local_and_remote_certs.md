---
title: Local & Remote SSL Certificates
date: 2025-10-19T09:00:00+10:00
draft: false
weight: 1
type: roadmap
summary: Co-op cloud can be configured to use both self-signed SSL certs for local domains, and real letsencrypt certificates for HTTPS on internet domains
---

{{< user_story >}}
_As a_ **Node Steward**
_I want to be able to_ **use both self-signed and real certificates for different domains on the same app**
_So that I can_ **make my apps work on the local network and the internet**
{{< /user_story >}}

## Co-op Cloud Traefik

Co-op Cloud traffic routing is done by Traefik, which is setup using the [recipe in this git repo](https://git.coopcloud.tech/coop-cloud/traefik).

Traefik creates routing rules for each app, based on the values in the app's `.env` file of `DOMAIN` and `EXTRA_DOMAINS`. Together, these result in a single traffic rule, with extra domains being a comma-seperated list of additional matching rules, not new traffic routes.

For each traffic route (and thus just once for each app) the default behaviour of the CC Traefik recipe is to generate a letsencypt certificate that covers all of the domains listed (in `DOMAIN` and `EXTRA_DOMAINS`).

## Local domains in Traefik

For `.local` domains that LoRes uses for serving apps on the local network, we can't use letsencrypt certificates. Instead, we need to use self-signed certificates.

The Traefik recipe accepts a single wildcard self-signed certificate. To use this, we create a certificate [in this step](/docs/node_stewards/initial_setup/coop_cloud/server/) and then enable wildcard certificates in the traefik config [in this step](/docs/node_stewards/initial_setup/coop_cloud/traefik/).

## The Problem

What this means, unfortunately, is that if we enable wildcard certificates then the current Traefik setup thinks that these should be used for all Traefik rules, and thus never generate letsencrypt certificates.

It would not be sufficient either, to create a new per-app config option like `USE_WILDCARD` or `USE_LETSENCYRPT` because what we want in LoRes is for each app to have both a `APPNAME-HOSTNAME.local` domain (using a self-signed cert) and an internet domain, using letsencrypt.

Instead, we might need an alternative to the `WILDCARDS_ENABLED` feature of the Traefik recipe, something that sets things up the way we need. Perhaps this could be a general feature that is useful for other applications in Co-op Cloud (our generally preferred approach), or perhaps it's a LoRes specific config that can be enabled in the Traefik recipe (which would probably be acceptable to Co-op Cloud given that we are part of the Federation).
