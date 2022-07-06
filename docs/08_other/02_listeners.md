---
title: Listeners
editor: markdown
sidebar_position: 2
---

Saffron supports listeners for various event. Listeners can be used for logging or creating analytics.

## Database

### `database.set.okay`
Arguments: `source, articles`

It is called when the articles are successfully uploaded to the database.

### `database.set.error`
Arguments: `source, error`

It is called when an error is occurred when uploading the articles.

### `database.get.error`
Arguments: `source, error`

It is called when an error is occurred when requesting articles from the database.

## Scheduler

### `scheduler.path.error`
Arguments: `error`

It is called if there is an error when reading the source file directory.
Common errors are the directory does not exist or there are insufficient permissions.

### `scheduler.sources.new`
Arguments: `sourceNames`

It is called when the scheduler loads the source files.

:::info
Note that this listener will be called after `includeOnly` and `exclude` configuration options are applied.
:::

### `scheduler.sources.error`
Arguments: `sourceFile, error`

It is called when the scheduler failed to load a source file.

### `scheduler.job.new`
Arguments: `jobId`

It is called when the scheduler pushes a new source scrapping job at the stack.

### `scheduler.job.finished`
Arguments: `jobId`

It is called when the scheduler finds a finished source scrapping job at the stack.

### `scheduler.job.failed`
Arguments: `jobId`

It is called when the scheduler finds a failed source scrapping job at the stack.

### `scheduler.job.reincarnate`
Arguments: `job`

It is called after the scheduler finds a failed source scrapping job and pushes it back to the stack.

### `scheduler.job.push`
Arguments: `job`

It is called after the scheduler finds a failed source scrapping job and pushes it back to the stack.

### `scheduler.job.worker.replace`
Arguments: `oldWorkerId, job`

It is called when scheduler finds a source scrapping job that has to been sent to the workers.


## Grid

### `grid.node.connected`
Arguments: `socket`

It is called when a new node is connected to the Grid.

### `grid.node.disconnected`
Arguments: `socket`

It is called when a node disconnects from the Grid.

### `grid.worker.announced`
Arguments: `workerId`

It is called when a worker is available to take source scrapping jobs.

### `grid.worker.destroyed`
Arguments: `workerId`

It is called when a worker stops being available to take source scrapping jobs.

## Workers

### `workers.job.finished`
Arguments: `job`

It is called when a worker has finished a source scrapping job.

:::info
Note there may be a max of `checksInterval` ms timeframe with the `scheduler.job.finished` event.
:::

### `workers.job.failed`
Arguments: `job`

It is called when a worker has failed a source scrapping job.

:::info
Note there may be a max of `checksInterval` ms timeframe with the `scheduler.job.failed` event.
:::

### `workers.articles.found`
Arguments: `articles, tableName`

It is called after a source scraping job has finished and before middleware has been called. It gives as parameters the
found articles from the website and the sourceName.

### `workers.articles.new`
Arguments: `articles, tableName`

It is called after saffron has checked with previous articles stored in the database.
It gives as parameters the new articles that will be uploaded to the database.

### `workers.parsers.error`
Arguments: `error`

It is called if a source scraping job has failed.

## Middleware

### `middleware.before`
Arguments: `articles`

It is called before middlewares are applied and after `workers.articles.found`. The difference between
the two is that `middleware.before` will not be called in case of zero articles.

### `middleware.after`
Arguments: `articles`

It is called after middlewares are applied and before `workers.articles.new`.

## Catch All
To catch all the events:

```javascript
saffron.on("*", (eventName, ...args) => {
    // ...
    console.log(args)
});
```