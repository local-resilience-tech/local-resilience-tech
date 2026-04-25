---
title: Choose IAM platform
date: 2026-04-16T01:00:00+00:00
draft: false
weight: 1
type: roadmap
summary: Choose which FOSS cloud app will be used for Identity and Access Management
params:
  status:
---

{{< hero >}}
The Identity and Access Management Platform we choose will be a core part of our users' experience within a LoRes Region.
{{< /hero >}}

Selection is carried out in the context of our [general section principles](../).

## General Required Features

- Single Sign On that is compatible with Co-op Cloud Apps (eg, OpenID)
- Assignment of apps to users
- Mobile-responsive design

## LoRes Specific Requirements

- Needs to synchronise between all nodes in the region. We don't expect this to happen out of the box, but we'd like a way to do this without forking an open source IAM product.

## Nice to have

- Assignment of users to groups/organisations
- Custom landing page
- A good user-sign up experience
- Dark mode

## Contenders

|              | [Rauthy](https://github.com/sebadob/rauthy) | [Kanidm](https://kanidm.com/) | [Authentik](https://goauthentik.io/) |
| ------------ | ------------------------------------------- | ----------------------------- | ------------------------------------ |
| **Backend**  | Rust 🦀                                     | Rust 🦀                       | Python 🐍                            |
| **Frontend** | Typescript & Svelte                         | Rust & server-side HTML       | Typescript & strangeness             |
| **CC Score** | 0                                           | N/A                           | 0                                    |
