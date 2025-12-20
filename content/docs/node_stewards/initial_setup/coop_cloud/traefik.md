---
title: The Traefik web proxy
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 2
---

{{<hero>}}
We're going to have multiple apps on our Raspberry Pi, so we need a way of directing traffic to the correct one. Co-op Cloud has a standard approach to this using a web proxy called Traefik.
{{</hero>}}

Despite Traefik's special role in routing requests to other apps, it is itself just installed as a Co-op Cloud app, managed by abra.

## Creating the app

Let's add the new app using:

```bash
abra app new traefik
```

It will ask you to select which server you're deploying to, unless this is your only server. It will also ask you to specify a domain for the app, and you can press ENTER for the default.

## Configuring the app

With the app added, you can now load it's config file using:

```bash
abra app config traefik.YOUR_SERVER_DOMAIN
```

(Remember that you can use autocomplete on the name, try typing `traefik` then hitting `<tab>`)

This should open the config file in your default text editor.

There is just one thing in this file that you need to change. The value of `LETS_ENCRYPT_EMAIL` needs to be changed to a real email address. You could use your personal address, or your local group could setup an address for this purpose. You can always change this later, so I'd go with personal address for now. Make that change and save the file.

## Deploying the app

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy traefik.YOUR_SERVER_DOMAIN
```

Then you can check that it has worked by directing a web browser at `https://traefik.YOUR_SERVER_DOMAIN`. The first time you try this, you'll probably seen an error in your browser like _"Secure Connection Failed"_. This is because the first time you load it, it goes off to fetch a SSL certificate. This can take anywhere from a few seconds to a minute. So just refresh that browser a few times and you should be right.

You should now be looking at the Traefik dashboard. It's a good tool for seeing how things work. We might disable it later for security reasons, but for now let's keep it there to explore.

## Pushing our changes to git

We made a bunch of changes to our server config in adding the traefik app, so let's commit those and push them to our upstream git server. Probably something like this:

```bash
cd .abra/servers/YOUR_SERVER_DOMAIN
git add .
git commit -m "added traefik"
git push origin
```
