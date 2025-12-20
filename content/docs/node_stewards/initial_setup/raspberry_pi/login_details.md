---
title: Your login details
date: 2025-09-03T09:00:00+10:00
draft: false
type: docs
weight: 1
---

{{< hero >}}
You're a **Node Steward**, perhaps not the only one who will help administer this **LoRes Node**. You're going to need some login credentials that you keep secure.
{{< /hero >}}

{{< figure src="/its-dangerous-to-go-alone-take-this.jpg" alt="An image of an old man in robes holding a sword, with the text - It's dangerous to go alone! Take this." caption="Nerd humour based on the 1986 game, The Legend of Zelda. Art by Nadeer.">}}

## A username and password

To log into this node, you're going to want a username and password. While it might be tempting to use the username `node_steward`, it's worth considering that there would ideally be multiple stewards of a node, to help keep things running smoothly. If someone stops volunteering as a node steward, we want to be able to easily remove them from the system. We might also want ways of logging which user does what, to provide transparency to our community.

So, pick a username that identifies you. In fact, to make life easier, we recommend you use the same username that you use on you **dev computer**. If you're just setting one of those up now, pick something short, memorable, lower-case, and ideally unique within your community of volunteers.

Now, choose a password that you're going to use for this user on this node. You're not going to use it often (we'll use another method to log in), just if you want to do manual system administration tasks (ie, use the `sudo` command). The best approach is to let your password manager generate a random password, and store it for when you need it.

## A SSH key-pair

Once it's ready, you'll be connecting to your server from your **dev computer** using [SSH](https://en.wikipedia.org/wiki/Secure_Shell) (Secure Shell Protocol). SSH uses key-pairs, which consist of a public key (known to the machine you're trying to connect with), and a private key (known only to you). The private key is like your password, if others get their hands on it, they can log in as you.

Given that, we recommend that you use a different key-pair for each dev computer. If you get a new computer, while you could copy the keys across, it's usually time to create a new one and retire the old one. This is to minimise the impact if someone managed to retrieve an old key of yours from a hard disk you thought you'd wiped properly.

### Generating the key-pair

To create your key-pair, open a terminal on your dev computer and run (replacing your email address):

```bash
ssh-keygen -t ed25519 -C "replace_this_comment"
```

For the comment, I like to use the format `username@hostname` so I remember which machine it was. For example, if my username is `aisha` and my computer's hostname is `wayfarer`, I'd run the command `ssh-keygen -t ed25519 -C "aisha@wayfarer"`. That comment will remind me where that key is from.

When you run this, you'll be asked where to save the file. You can press _Enter_ to accept the default file location (something like `/home/aisha/.ssh/id_ed25519`), or if you have multiple keys you can give it a custom name (but you'll need to type out the full path you want, to keep it in that same `.ssh` folder).

### Setting the passphrase

Next you'll be asked to set a passphrase. You can press _enter_ for no passphrase, but that's not a good idea. As mentioned above, if someone was to get your private key file, they'd have access to all your stuff. Setting a passphrase is a good way to ensure that you need both the file, and this bit of information. Despite the name, the passphrase is essentially a password. You can use your password manager to generate this. Perhaps create a login item in your password manager called something like `ssh key aisha@wayfarer` allow it to generate a passphrase, and paste that in your terminal.

{{<aside>}}
💡 **Hint:** if you're using Ubuntu Linux, pasting in your default terminal requires you to press SHIFT-CONTROL-V. If you leave off the SHIFT, you'll paste some other characters too. If you're not sure, right-click and use the mouse. More details [here](https://www.howtogeek.com/440558/how-to-copy-and-paste-text-at-linuxs-bash-shell/).
{{</aside>}}

## Adding your SSH key to the ssh-agent

Using a passphrase every time you use your SSH key would be brutal, so there's a tool called `ssh-agent` that asks for the passphrase on first use, decrypts your key, and lets you use it automatically for the rest of your session on that computer. If you log out and in again, or reset, you might need to enter you passphrase again, which seems appropriate.

On Linux, ssh-agent should already be running, and you just need to add this new key using:

```bash
ssh-add ~/.ssh/id_ed25519
```

You'll be prompted for your passphrase to do this.

## Wrapping up

We're going to use both the password and the key-pair we just created in setting up our Raspberry Pi. The key-pair can also be used for other things, you usually only need one of those for your dev computer to prove that you are _you_. Let's proceed to setup our Pi and use those credentials.
