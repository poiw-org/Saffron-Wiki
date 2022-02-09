---
title: Introduction
editor: markdown
sidebar_position: 1
---


**Saffron | News & announcements aggregation framework**

The project is maintained by [po/iw](https://poiw.org) (ποιώ), a Greek university team that supports Free/Libre
and Open Source Software and hardware. Saffron was built to help teams collect and compile huge lists of aggregated
news and announcement content intuitively and efficiently.

## Architecture

Saffron's architecture is based on a `main` node that issues scraping instructions and several `worker` nodes
that do the scraping & upload the data to the database.

The communication between the nodes is happening through the `Grid`. By using a [`socket.io`](https://socket.io)
implementation and in app encryption system, the communication is end to end safe.


## Versions

### 2.1.0
* New features
  * Added event listeners `middleware.before` & `middleware.after`
  * Added log levels `info` & `errors`

### 2.0.0

* Changes
  * Reformatted configuration fields.
  * Reformatted source fields.
  
* New features
  * Added more event listeners
  * Logs can be switched off.


### 1.x.x

Version `1.x.x` was created for test purposes, and it is unstable. It recommended using any version above `2.0.0`.