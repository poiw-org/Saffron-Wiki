---
title: Configuration
published: true
date: 2021-11-15T19:04:32.101Z
editor: markdown
dateCreated: 2021-07-19T05:08:46.716Z
sidebar_position: 2
---

# Configuration
Each saffron instance need a configuration file. Below is the structure of the configutration file.



### Database configuration
```json
"database": {
  "driver": "memory", // Default driver
  "config": {}
}
```
* `driver` this field contains the name of the database driver that will be used for storing the articles
  Available drivers: *mongodb*, *memory*, *none*

* `config` contains the configuration for each driver
    * mongodb:
  ```json
	"config": {
    "url": "", // The mongodb url
    "name": "" // The database name where the collections will be created.
  }
	```
    * memory: No extra configuration needed.
    * none: No extra configuration needed (The articles will not be saved in any form of database).

### Source files
This field is necessary only `mode`: main (check below).
```json
"sources":{
  "path": "./sources", // The default path
  "includeOnly": [],
  "excluded": []
}
```
* `path` is the source file folder path.
* `includeOnly` is an array that will includeOnly selected sources based on their name.
* `excluded` is an array that will exclude the sources based on their name. It will still exclude articles if they match with items in `includeOnly` field.

### Mode and workers

```json
"mode": "main",
"workers": {
  "nodes": 1,
  "job": {
    "requestTimeout": 5000
  }
},
```
* `mode` decides if is the main inctance of saffron or a worker instance. Put "main" for main saffron and "worker" for worker instances only.
* `workers`->`nodes` how many workers will be started with this saffron instance (`mode` has no effect here).



### Scheduler and Grid
```json
"scheduler": {
  "intervalBetweenJobs": 3600000,      // Default: 1 hour
  "heavyJobFailureInterval": 86400000  // Default: 1 day
  "intervalBetweenChecks": 120000      // Default: 2 minutes
},
"grid": {
  "distributed": false
}
```
* `intervalBetweenJobs`: The default interval between the source file scraping jobs. It can be overwriten for each source file.
* `heavyJobFailureInterval`: this interval will be used once after a source file fails 10 times in a row.
* `intervalBetweenChecks`: this interval is used from the scheduler to periodically check the jobs' status. If `intervalBetweenJobs` is smaller than `intervalBetweenChecks` then `intervalBetweenJobs` will be overrided with the others value.

All intervals are in milliseconds format.





### Log level
```json
"misc": {
  "log": "all"
}
```
The log level that a saffron instance will print. Available options: *info, debug, error, exception*
* `all`: logs everything.
* `none`: logs nothing.
