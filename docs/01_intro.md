---
title: Introduction
editor: markdown
sidebar_position: 1
---


**Saffron | News & announcements aggregation framework**

The project is maintained by [po/iw](https://poiw.org) (ποιώ), a Greek university team that supports Free/Libre
and Open Source Software and hardware. Saffron was built to help teams collect and compile huge lists of aggregated
news and announcement content intuitively and efficiently.

### Architecture

Saffron's architecture is based on a `main` node that issues scraping instructions and several `worker` nodes
that do the scraping & upload the data to the database.

The communication between the nodes is happening through the `Grid`. By using a [`socket.io`](https://socket.io)
implementation and in app encryption system, the communication is end to end safe.
