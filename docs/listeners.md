---
title: Listeners
published: true
date: 2021-10-02T14:00:45.981Z
editor: markdown
dateCreated: 2021-08-07T09:30:05.229Z
sidebar_position: 5
---

# Listeners

Saffron supports listeners for each event that has a meaning.

Below is a list of all the events:

```typescript
// The names of all the validated sources (without the excluded).
saffron.on("scheduler.sources.new", (names: string[]) => ...)

// A error occurred during source file parsing.
saffron.on("scheduler.job.new", (sourceFile: any, error: any) => ...)
// A new job ha been put on stack.
saffron.on("scheduler.job.new", (job: Job) => ...)
// The time for the job to be runned has come.
saffron.on("scheduler.job.push", (job: Job) => ...)
// A finished job has been found.
saffron.on("scheduler.job.finished", (job: Job) => ...)
// A failed job has been found.
saffron.on("scheduler.job.failed", (job: Job) => ...))
// A failed job has been rescheduled.
saffron.on("scheduler.job.reincarnate", (job: Job) => ...)
// A worker has been replaced due to incossistence of delivering a job.
saffron.on("scheduler.job.worker.replace", (old_worker_id: string, job: Job) => ...)

// A new worker has been registered.
saffron.on("grid.worker.announced", (worker_id: string) => ...)
// A worker has been unregistered.
saffron.on("grid.worker.destroyed", (worker_id: string) => ...)

// A worker has finished a job.
saffron.on("workers.job.finished", (job: Job) => ...)
// A worker has failed a job.
saffron.on("workers.job.failed", (job: Job) => ...)
// Failed to upload article to database.
saffron.on("workers.articles.errorOffloading", (article: Article) => ...)

// The articles a worker has downloaded.
saffron.on("workers.articles.found", (articles: Article[]) => ...)
// The new articles that will be added to the database
saffron.on("workers.articles.new", (articles: Article[]) => ...)

// A parser encountered an error.
saffron.on("workers.parsers.error", (data: any) => ...)

// All the logs will also be sent here.
saffron.on("log", ({type, log}) => ...)
```


This page is maintained by: [JexSrs](https://github.com/JexSrs)
