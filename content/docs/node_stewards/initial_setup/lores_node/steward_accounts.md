---
title: Node steward accounts
date: 2026-03-08T09:00:00+10:00
draft: false
type: docs
weight: 2
menus:
  docs:
    name: Steward accounts
    parent: LoRes Node
---

{{<hero>}}
Each Node Steward of your node gets their own account on lores-node, to allow for collaboration and transparency.
{{</hero>}}

However, in this initial setup, that's probably just you. Let's go through the process of setting up an account for you to administer the node.

## Storing the admin credentials

Presuming that you've just [installed lores-node with Co-op Cloud](..), you should now be able to visit it's url in your browser at `https://YOUR_HOSTNAME.local`, and you'll see the following:

{{< figure src="/images/node_stewards/lores_node/setup-admin-password.png" alt="A screenshot with the text: Setup your admin password. The admin password is only used to create the users you use to steward this node. It can be reset at any time. The password is auto-generated and only displayed to you this once. If you're ready to store it in a safe place (an encrypted password manager, for example), click the button below to continue.">}}

When you're ready to get started, go ahead and hit "Generate admin password", and when you're given the admin password, store that somewhere in password manager.

## Creating your Node Steward account

To manage accounts for **Node Steward**, you log into the admin interface at `https://YOUR_HOSTNAME.local`. This just requires the admin password you already saved.

On that screen, hit the plus (`+`) button and give your new node steward a display name. That doesn't have to be unique, there's a unique ID generated for each steward, but just a name that makes sense amongst your fellow Node Stewards.

{{< figure src="/images/node_stewards/lores_node/new-node-steward.png" alt="A screenshot with the text: New node steward. We identify node stewards with a unique ID, rather than an email address, in case email verification is not possible. We'll create that ID for you, and display a temporary access code that the you can give to the node steward to log in for the first time and set their password.">}}

You will then see a screen with temporary login details for the new node steward. These consist of an ID, and a temporary access code, which can be used once to login and set a password.

Node Stewards are often added in person, and maybe even when the internet is down, so we tried to keep these codes fairly short, so that you can just read them from one screen and type into another.

In this case, we're creating a Node Steward account for ourselves, so it's probably easiest to hit the "Copy details to clipboard" button, and paste these details temporarily into a text editor for the next step.

## Setting your Node Steward password

To set your password, go back to the main page at `https://YOUR_HOSTNAME.local` and hit "log in".

On the log in page, you'll notice that it says "If you are a new node steward, you will have been given a one-use token to set a password.", and `to set a password` is a link to `/auth/node_steward/set_password`. Go to that page and pop in your node steward ID, your one-use token, and your new password.

Of course you're no doubt using a password manager for this, generating a strong password, and storing it with the ID. You wont need the one-use token again.

After that, you can go ahead and log in as normal.
