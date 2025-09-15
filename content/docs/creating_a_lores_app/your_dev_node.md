---
title: Setting up your local LoRes Node
date: 2025-09-03T09:00:00+10:00
draft: false
weight: 5
type: docs
---

[Previously](../your_dev_environment) we covered getting LoRes Node up and running on your development machine in a way that would automatically load the **App Repo** you're working on. With a new node, there's a little bit of setup needed to get everything up and running, so let's go through that quickly.

To do this, you should have the node running at [http://localhost:8200](http://localhost:8200).

## Admin User Setup

When you visit [LoRes Node](http://localhost:8200) you should automatically get redirected to the page entitled _"Setup your admin password"_. You need an admin password to create users. Go ahead and hit the _"Generate admin password"_ button and save the password it creates in your password manager.

Then, you can go ahead and log in as the Admin user using this password.

## Node Steward Setup

### As Admin, Create a Node Steward

You need to create a non-admin user to actually do anything. This type of user is called a **Node Steward**. When you're logged in as admin you should be on a Node Steward's page, which says _"No node stewards found"_. Hit the `+` (plus) button on the right of the page to take you to the new node steward form, and set a name for that steward (maybe just your own name).

This will present you with a _Node Steward ID_ and a _Temporary Access Code_. Hit the button to copy all those details to the clipboard. I like to open a text editor for the moment and paste those details into a document. We only need that access code for a moment, so there's no sense in putting it in a password manager.

### Log in as Node Steward

Go back to the [Lores Node home page](http://localhost:8200) and hit the log in button. From the login page click on the link _"to set a password"_ (or just click [here](http://localhost:8200/auth/node_steward/login)).

On that page, enter the _Node Steward ID_ and _Temporary Access Code_ that you just pasted into a text document. And, pick a new password (maybe generate one using your password manager). At this point you want to save the ID and new password in your password manager (you wont need the temporary access code again).

With that done, you can go ahead and login as that Node Steward user.
