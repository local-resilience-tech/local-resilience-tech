---
title: The Traefik web proxy
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 3
type: docs
menus:
  docs:
    name: Web proxy
    parent: Co-op Cloud
---

{{<hero>}}
We're going to have multiple apps on our Raspberry Pi, so we need a way of directing traffic to the correct one. Co-op Cloud has a standard approach to this using a web proxy called Traefik.
{{</hero>}}

Despite Traefik's special role in routing requests to other apps, it is itself just installed as a Co-op Cloud app, managed by abra.

## Creating the app

Let's add the new app using:

```bash
abra app new traefik --server YOUR_HOSTNAME.local --domain traefik-YOUR_HOSTNAME.local
```

(Don't forget to replace `YOUR_HOSTNAME` with the hostname of your pi.)

## Configuring the app

With the app added, you can now load it's config file using:

```bash
abra app config traefik-YOUR_HOSTNAME.local
```

(Remember that you can use autocomplete on the name, try typing `traefik` then hitting `<tab>`)

This should open the config file in your default text editor.

### Enabling local certificates.

Find the section in the file headed with `Manual wildcard certificate insertion`

In that section, you need to "un-comment" the following lines by removing the `#` symbol at the start of each line.

```env
#WILDCARDS_ENABLED=1
#SECRET_WILDCARD_CERT_VERSION=v1
#SECRET_WILDCARD_KEY_VERSION=v1
#COMPOSE_FILE="$COMPOSE_FILE:compose.wildcard.yml"
```

{{<aside>}}

### Commenting / Uncommenting

Programming languages, and even configuration files, often have a special character that denotes that a line of text isn't for the computer to read, it's just a comment left in for humans.

Programmers also often use this feature to temporarily disable a section of the file. This is process of making code into a comment is called _"commenting"_ or _"commenting it out"_. The process of undoing that is called _"uncommenting"_.

{{</aside>}}

## Adding the certificates to the docker secrets

In the [last step](../server) we created a pair of certificate files for our server. This is where we add them.

Run the following from your dev machine:

```bash
abra app secret insert traefik-radish.local ssl_cert v1 pi_local.crt -f
abra app secret insert traefik-radish.local ssl_key v1 pi_local.key -f
```

## Deploying the app

Deploy this change to your Raspberry Pi by running:

```bash
abra app deploy traefik-YOUR_HOSTNAME.local --no-domain-checks
```

## Browsing to the Traefik dashboard

If everything went well, we should have Traefik installed. We can check if it worked by going to the following URL in our browser, `https://traefik-YOUR_HOSTNAME.local`.

But wait, what's this??

{{< figure src="/images/node_stewards/initial_setup/traefik/be_careful.png" alt="A screenshot of Firefox, at the address traefik-radish.local, with a warning which says: Be careful. Something doesn't look right. Firefox spotted a potentially serious security issue with traefik-radish.local. Someone pretending to be the site could try to steal things like credit card info, passwords, or emails.">}}

We've hit the problem that our **self-signed certificate** isn't a real certificate trusted by some internet authority, and our browser is here to protect us. When we use our server on the internet, we're going to use a real certificate, but for now anyone using this site on your local network is going to have to proceed here despite the scary warning of risk. To do so, hit the _**"Advanced"**_ button.

{{< figure src="/images/node_stewards/initial_setup/traefik/advanced.png" alt="A screenshot of Firefox, the same as the previous image with the advanced section displaying. It says: What makes the site look dangerous? Because there’s an issue with the site’s certificate. Sites use certificates issued by a certificate authority to prove they’re really who they say they are. This site’s certificate is self-signed. It wasn’t issued by a recognized certificate authority – so we don’t trust it by default. What can you do about it? Not much. It’s likely there’s a problem with the site itself. IMPORTANT NOTE: If you are trying to visit this site on a corporate intranet, your IT staff may use self-signed certificates. They can help you check their authenticity.">}}

With the advanced section expanded, hit the button that says _**"Proceed to traefik.YOUR_HOSTNAME.local (Risky)"**_.

And with that, you should be in. If it worked, ou should now be looking at the Traefik dashboard. It's a good tool for seeing how things work. We might disable it later for security reasons, but for now let's keep it there to explore.

{{< figure src="/images/node_stewards/initial_setup/traefik/dashboard.png" alt="A screenshot of the web dashboard for Traefik proxy, showing entry-points on port 80 and 443.">}}
