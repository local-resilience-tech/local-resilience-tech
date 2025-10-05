---
title: Allowing configuration by Node Stewards
date: 2025-10-04T09:00:00+10:00
draft: false
type: docs
---

{{< hero >}}
This section is optional.
{{< /hero >}}

Your **Lores App** may just work one particular way, or it may allow configuration by users of the app with that config set somewhere in the app's internal data.

However, if you want your app to be configured by **Node Stewards** who are using the **Lores Node** user interface, we have a way to do that. You have to specify what configuration options are allowed, and Lores Node will display UI to allow the config to be edited. When your app is mounted in docker, the configuration will be available to you in a JSON file, for you to do what you like with.

{{<aside>}}

### JSON

JSON stands for _"JavaScript Object Notation"_. It's a way writing data as text, in the same way that it would be written in the JavaScript programming language. However, it contains no JavaScript code, it can't _do anything_. You can see some examples [here](https://www.w3schools.com/whatis/whatis_json.asp).

JSON became popular because it is simple, human readable, and widely understood as it's used all over the internet. It's not the most concise option for machine storage, and it's not always the best option for simple human-written configuration because it requires the user to get matching opening and closing symbols correct.
{{</aside>}}

## Configuration Schema

The configuration schema defines what configuration options you accept. It's defined using **JSON Schema**.

{{<aside>}}

### JSON Schema

In programming, the word _"Schema"_ generally means the definition of what the data can look like. Such as defining that a given database of people can store their _name_ as a string and their _birthday_ as a date.

[JSON Schema](https://json-schema.org/) is a schema system for JSON. It allows you to specify what types of values can be in a JSON file. The file can then be validated against the schema to see if it matches. It can even specify validations on the data (eg, _age_ is a number but it can't be under 18).

A JSON Schema is itself stored as a JSON file. Since of course you can only put things in that file that make sense as a JSON Schema, there is a _"meta schema"_ file that defines the schema for a JSON Schema file. There have been different versions of that release, our examples are based on [draft 7](https://json-schema.org/draft-07),
{{</aside>}}

To provide a configuration schema for your app, you essentially just write a JSON Schema file and put it in the root of your Lores App, and most critically you must name it `config_schema.json`.

It really is as simple as that. If you need help writing your schema there are lots of tools available on the web. For example:

- [here's a schema validator](https://www.jsonschemavalidator.net/) that has examples and will let you know if you've made any mistakes
- [here's a schema generator](https://www.jsongenerator.io/schema) that will build a schema from your example JSON file!
- [here's a UI playground](https://rjsf-team.github.io/react-jsonschema-form/) that takes your schema and builds a user-interface (it's the library we use in **Lores Node**, so you can get an idea of what it will look like).

Once you've got that file, if you put the config_schema.json in app and install it in your local version of LoRes Node, then login as a Node Steward and go to the page for that app, you should see a **Configure** button. Hit that, and you should be on the page to edit your schema for the installed app.

## Mounting Config Volume

So when a Node Steward changes the config for an installation of your app, we need a way for that configuration JSON to be available to the app. To do that, we need to mount a volume in docker that is used for configuration. It's pretty simple and we'll walk through what to do together, but the more detailed documentation is [this page on volumes in docker compose](https://docs.docker.com/reference/compose-file/volumes/).

### Specify the volume mount on each service

Your docker compose file includes a list of services (perhaps only one). Do the following for each of your services that need access to the config file (it might not be all of them).

Inside the named service in the compose file, add a `volumes:` section (if it isn't already there), and add a volume mount that puts the volume named `lores_config` somewhere in the local filesystem. We recommend `/lores/config` but it can be anywhere you like. That means you should have a volumes section something like this:

```yaml
services:
  my_service:
    volumes:
      - lores_config:/lores/config
```

### Add the volume to the requirements for your compose file

At the top level of your docker compose file is a set of volume declarations for named volumes that can be re-used between services. You need to add `lores_config` there too, so that **Lores Node** knows to supply that volume to your app. That looks something like:

```yaml
volumes:
  lores_config:
```

### Putting it all together

If you've done all that, your app should have a compose file that looks a bit like:

```yaml
version: "3.8"
services:
  web:
    image: some/image
    networks:
      - lores
    volumes:
      - lores_config:/lores/config
volumes:
  lores_config:
networks:
  lores:
    external: true
```
