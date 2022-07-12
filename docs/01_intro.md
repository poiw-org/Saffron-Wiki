---
title: Introduction
editor: markdown
sidebar_position: 1
---


**Saffron | News & announcements aggregation framework**

**S**imple **A**bstract **F**ramework **F**or the **R**etrieval **O**f **N**ews

The project is maintained by [po/iw](https://poiw.org) (ποιώ), a Greek university team that supports Free/Libre
and Open Source Software and hardware. Saffron was built to help teams collect and compile huge lists of aggregated
news and announcement content intuitively and efficiently.

## What is this?
Saffron is an abstraction engine that helps you collect news and announcements from websites
in a uniform way. All you have to do is write a file for each website you want to be scraped
and run it in your NodeJS app. Saffron manages the scraping interval, data parsing
and - if you want to - the data offloading to your database of choice.

## Architecture

Saffron's architecture is based on a `main` node that issues scraping instructions and several `worker` nodes
that do the scraping & upload the data to the database.

The communication between the nodes is happening through the `Grid`. The grid will generate events to communicate
with other classes. Saffron supports remote nodes by using [`socket.io`](https://socket.io) server and clients
as a middleware to connect to the `main` node.


## Versions

### 3.0.1
* Fixes
  * Issue new workers for imported jobs.

### 3.0.0
* New features
  * Type support for configuration.
  * Changed embedded database to user-end implementation.
  * Finished Grid.
  * More than one category per url.
  * Added debug tools.
  * Allow scheduler to continue from previous session.
  * Grid authorization using tokens.
  * Saffron will abort the job if a middleware is rejected with an error.
  * One extension can be used multiple times.
  * Support for re-reading the source from file system without interrupting the scheduler.
* Changes
  * Code remodelling.
  * Removed hash field from article.
  * Merged serialization functions.
  * Grid supports HTTPS instead of in-app encryption.
  * Removed redundant code.
  * SourceException error messages.
  * Saffron is remodeled in a class.
  * Listeners `workers.job.finished`, `workers.job.failed` and `workers.job.failed` emit the job's id and not the job class anymore.
  * Removed listeners `database.connection.okay`, `database.connection.failed` and `workers.articles.errorOffloading`.
  * Added listener `database.set.okay`, `database.get.error`, `database.set.error` and `middleware.error`.
  * Job handling is done by the Scheduler (previously was Grid).
  * Unified parsers' arguments in the util class.
  * Dynamic parser will receive both initialized utils and uninitialized Article class.
  * Axios was abstracted inside the utils class.
  * `parse` function will return an array with articles per url instead of a merged array.
* Fixes
  * Listener `middleware.after` not called on database `none`.
  * Grid will not emit to remote clients unnecessary events.

### 2.2.x
* Changes
  * Remodeled WordPress parser.
* Fixes
  * WordPress parser did not put url-category at the categories list.

### 2.1.x
* New features
  * Allow custom encoding using TextDecoder at axios response.
* Fixes
  * Dynamic parser missing article id.
  * HTML parser fixed attachment list pushed as one attachment.

### 2.1.0
* New features
  * Added event listeners `middleware.before` & `middleware.after`.
  * Added log levels `info` & `errors`.

### 2.0.0

* Changes
  * Reformatted configuration fields.
  * Reformatted source fields.
  
* New features
  * Added more event listeners
  * Logs can be switched off.


### 1.x.x

Version `1.x.x` was created for test purposes, and it is quite unstable. It is recommended that you're using any version above `2.0.0`.
