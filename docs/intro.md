---
title: Home
published: true
date: 2021-09-17T11:34:50.093Z
editor: markdown
dateCreated: 2021-05-23T08:39:26.227Z
sidebar_position: 1
---

# Introduction
**Saffron | News & announcements aggregation framework**

The project is maintained by po/iw (ποιώ), a Greek university team that supports Free/Libre and Open Source Software and hardware.
Saffron was built to help teams collect and compile huge lists of aggregated news and announcement content intuitively and efficiently.

### Architecture

Saffron's architecture is based on a `main` node that issues scraping instructions and several `worker` nodes that do the scraping & upload the data to the database.

![diagram.svg](/diagram.svg)

The communication between the nodes is happening through the `Grid`. By using a `socket.io` implementation and in app encryption system, the communication is end to end safe.

### Installation

**To use saffron, you will need to import it from a separate NodeJS App.**

To install it, use:
`npm i @poiw/saffron` or - if you are fancy: `yarn add @poiw/saffron`.

Then, you can import it using:
``` js
// ES6
import saffron from "@poiw/saffron"

// ES5
const saffron = require("@poiw/saffron")
```
Finally, initialize saffron using:
``` js
// Write your desired configuration in a `saffron.json` 
// file and save it in your project's root folder.
await saffron.initialize()

// or declare a path that points to your configuration file
await saffron.initialize("path/to/config.json")

// or pass a JSON object directly
await saffron.initialize({ ... })
```

See also: [How to create a configuration file](https://saffron.poiw.org/en/configuration)



### Usage

...

You can also use the one time parse function to parse a specific source.
``` js
// The source file in json format.
let source_json = { ... };

// Parse the source.
let articles = await saffron.parser(source_json);
console.log(articles);
```
If an error occur while parsing (e.x. unreachable site) then an object with the error will be returned.