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
