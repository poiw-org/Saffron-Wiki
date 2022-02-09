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

### `driver`
Default value: `none`

The database driver where the parsed articles will be stored.
The available drivers at the moment are: `none`, `memory` and `mongodb`.

### `config`
Default value: `{}`

The needed configuration a database driver needs. The drivers `none` and `memory` do not require custom configuration.

<Tabs groupdId="dbdriver">
<TabItem value="mongodb" label="mongodb">

```js
let config = {
    url: "MOGNO_URL",
    name: "DATABASE_NAME"
}
```

</TabItem>
</Tabs>


## Sources

### `path`
Default value: `./sources`

The folder where the source files are located (will also search for sub folders).

### `includeOnly`
Default value: `[]`

This field was created mainly for test purposes when we want to include only specific sources.
The source identification is made using the source name (not the source file name).

### `exclude`
Default value: `[]`

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


## Workers

### `nodes`
Default value: `1`

The worker nodes that will be started with the saffron instance. In case of multiple workers,
each worker will run on different thread. This is useful in case of a large file of source files.

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

:::info
If `mode` is set to `worker` it will search for the `main` grid.
:::


## Misc
### `log`
Default value: `all`

The saffron log level. Can be set as `all`, `info`, `errors` or `none`.


## Example

```js
let oonfig = {
    database: {
        driver: "none",
        config: {}
    },
    sources: {
        path: "./sources",
        includeOnly: [],
        exclude: []
    },
    mode: "main",
    workers: {
        nodes: 1,
        jobs: {
            timeout: 10000,
        },
        articles: {
            amount: 10
        }
    },
    scheduler: {
        jobsInterval: 3600000,
        heavyJobFailureInterval: 86400000,
        checksInterval: 120000
    },
    grid: {
        distributed: false
    },
    misc: {
        log: "all"
    }
}
```