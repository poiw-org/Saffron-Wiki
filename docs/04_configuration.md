---
title: Configuration
editor: markdown
sidebar_position: 4
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Each saffron instance need a configuration file. Below is the structure of the configuration file.

## Mode
Default value: `main`

Can receive the values `main` and `worker`. It specifies if the saffron instance will be the main instance
or a worker only instance.

## Database
The database functions that will be called from Saffron. They need to be initialized from the developer.
Any kind of error thrown inside the database functions will be called by the Saffron and abort the uploading.

### `pushArticles`
Takes as argument an array of new articles that needs to be pushed in the database.

### `getArticles`
Takes as argument an object containing the fields: `tableName`, `count`.
* `tableName`: The table (or collection) where the articles will be retrieved
* `count`: How many articles will be retrieved from the database.

## Sources

### `path`
Default value: `./sources`

The folder where the source files are located (will also search for sub folders).

### `includeOnly`
This field was created mainly for test purposes when we want to include only specific sources.
The source identification is made using the source name (not the source file name).

### `exclude`
This field was created mainly for test purposes when we want to exclude specific sources.
The source identification is made using the source name (not the source file name).

:::info
This option will exclude sources from `includeOnly` if it is set.
:::


## Scheduler

### `jobsInterval`
Default value: `3600000` (60 minutes)

The default interval between the source scrapping jobs (it can be overwritten for each source).
This option is also used by the scheduler to spread the source files evenly inside a specific timeframe.

:::caution
The value should not be lower than `checksInterval`.
:::

### `heavyJobFailureInterval`
Default value: `86400000` (24 hours)

There are cases where a source scrapping job will continuously fail (website is down).
After `10` continuous fail attempts it will freeze the job and schedule it after `heavyJobFailureInterval` ms.

### `checksInterval`
Default value: `120000` (2 minutes)

The scheduler checks periodically to find if a source scrapping job has finished or failed.
This option is the interval between these checks. 

### `randomizeInterval`
Default value: `-500 to 500 seconds`

The scheduler adds/subtracts a random time from the interval of each job to avoid making
requests at fixed intervals. The result of the function must be in milliseconds.

```javascript
randomizeInterval: () => {
    const random = Math.floor(Math.random() * (high - low) + low) * 1000;
    return Math.random() >= 0.5 ? random : -random;
}
```

## Workers

### `nodes`
Default value: `1`

The worker nodes that will be started with the saffron instance. In case of multiple workers,
each worker will run on different thread. This is useful in case of a large file of source files.

### `userAgent`
The User-Agent that will accompany the request.

### `jobs.timeout`
Default value: `10000`

The source scrapping job request timeout (it can be overwritten for each source).

### `articles.amount`
Default value: `10`

The amount of articles that will be parsed for each source scrapping job.

## Grid
### `distributed`
Default value: `false`

If the grid network will be started or not.

### `useHTTP`
Default value: `false`

Use HTTP instead of HTTPS.

### `serverAddress`
The server's address without the `http://` or `https://` prefix.
The address is the same as the `main` node's address.

### `serverPort`
Default value: `3000`

The port where the grid will listen.

### `authToken`
The token that will authenticate the `worker` nodes with the `main` node.

### `key`
The key used by the HTTPS server.

### `cert`
The certificate used by the HTTPS server.

## Misc
### `log`
Default value: `all`

The saffron log level. Can be set as `all`, `info`, `errors` or `none`.

# Environment
Saffron supports different configuration based on the environment.

The configuration will first receive the root configuration:
```javascript
{
    databsase: {
        pushArticles: (articles) => {
            // push to production database
        },
        // ...
    },
    misc: {
        log: "errors" // log only errors
    },
    // ...
}
```

and then read the child object `development`, `production` or `testing` based on the environment:

```javascript
{
    databsase: {
        pushArticles: (articles) => {
            // push to production database
        },
        // ...
    },
    misc: {
        log: "all"
    },
    // ...
    development: {
        databsase: {
            pushArticles: (articles) => {
                // push to local database
            },
            // ...
        },
        misc: {
            log: "all" // log everything
        },
        // ...
    }
}
```
