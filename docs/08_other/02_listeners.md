---
title: Listeners
editor: markdown
sidebar_position: 2
---

Saffron supports listeners for various event. Listeners can be used for logging or creating analytics.

## Database

### `database.driver.error`
Arguments: none

It is called if the database driver during the saffron initialization is invalid.

### `database.connection.okay`
Arguments: none

It is called when the connection with the database is established.

### `database.connection.failed`
Arguments: none

It is called when the connection with the database has failed.

### `database.operation.error`
Arguments: `(funcname: string, error: any)`

It is called when an error is occurred during a database operation like uploading or retrieving articles.

## Scheduler

### `scheduler.path.error`
Arguments: `(error: any)`

It is called if there is an error when reading the source file directory.
Common errors are the directory does not exist or there are insufficient permissions.

### `scheduler.sources.new`
Arguments: `(sourceNames: string[])`

It is called when the scheduler loads the source files.

:::info
Note that this listener will be called after `includeOnly` and `exclude` configuration options are applied.
:::

### `scheduler.sources.error`
Arguments: `(sourceFile: any, error: any)`

It is called when the scheduler failed to load a source file.

### `scheduler.job.new`
Arguments: `(job: Job)`

It is called when the scheduler pushes a new source scrapping job at the stack.

### `scheduler.job.finished`
Arguments: `(job: Job)`

It is called when the scheduler finds a finished source scrapping job at the stack.

### `scheduler.job.failed`
Arguments: `(job: Job)`

It is called when the scheduler finds a failed source scrapping job at the stack.

### `scheduler.job.reincarnate`
Arguments: `(job: Job)`

It is called after the scheduler finds a failed source scrapping job and pushes it back to the stack.

### `scheduler.job.push`
Arguments: `(job: Job)`

It is called after the scheduler finds a failed source scrapping job and pushes it back to the stack.

### `scheduler.job.worker.replace`
Arguments: `(oldWorkerId: string, job: Job)`

It is called when scheduler finds a source scrapping job that has to been sent to the workers.


## Grid

### `grid.connection.okay`
Arguments: none

It is called when Grid system becomes online on the network.

### `grid.connection.failed`
Arguments: `(error: any)`

It is called when Grid system fails to connect to the network.

### `grid.node.connected`
Arguments: none

It is called when a new node is connected to the Grid.

### `grid.node.disconnected`
Arguments: none

It is called when a node disconnects from the Grid.

### `grid.worker.announced`
Arguments: `(workerId: string)`

It is called when a worker is available to take source scrapping jobs.

### `grid.worker.destroyed`
Arguments: `(workerId: string)`

It is called when a worker stops being available to take source scrapping jobs.


## Workers

### `workers.job.finished`
Arguments: `(job: Job)`

It is called when a worker has finished a source scrapping job.

:::info
Note there may be a max of `checksInterval` ms timeframe with the `scheduler.job.finished` event.
:::

### `workers.job.failed`
Arguments: `(job: Job)`

It is called when a worker has failed a source scrapping job.

:::info
Note there may be a max of `checksInterval` ms timeframe with the `scheduler.job.failed` event.
:::

### `workers.articles.errorOffloading`
Arguments: `(article: Article)`

It is called when database fails to upload a specific article.

### `workers.articles.found`
Arguments: `(articles: Article[], sourceName: string)`

It is called after a source scraping job has finished and before middleware has been called. It gives as parameters the
found articles from the website and the sourceName.

### `workers.articles.new`
Arguments: `(articles: Article[])`

It is called after saffron has checked with previous articles stored in the database.
It gives as parameters the new articles that will be uploaded to the database.

### `workers.parsers.error`
Arguments: `(error: any)`

It is called if a source scraping job has failed.


## Middleware

### `middleware.before`
Arguments: `(articles: Article[])`

It is called before middlewares are applied and after `workers.articles.found`. The difference between
the two is that `middleware.before` will not be called in case of zero articles.

### `middleware.after`
Arguments: `(articles: Article[])`

It is called after middlewares are applied and before `workers.articles.new`.


## Catch All
```ts
saffron.on("*", (...args: any[]) => {
    // ...
    console.log(args)
})
```