---
title: General
editor: markdown
sidebar_position: 1
---

## What is a source file?

A source file is a `json` or `javascript` file that represents a website.
These files are generated from the user and guide Saffron on how to parse a website.

Each parser utilizes a different source file structure but there are some common options.

## Common options

### `name`
This field identifies the source file.
Although saffron does not check if the name is unique it is required to be so.

It is also used in configuration [`includeOnly`](../configuration#includeonly)
and [`exclude`](../configuration#exclude).

### `tableName`
Default value: [`name`](#name)

When requesting (or saving) articles from (to) the database, saffron will send the tableName as the path
to where the articles are located. This is useful in case of multiple source files want to save
at the same place.

If it is not defined it will fallback to the source name field.

### `interval`
Default value: `3600000`

The time the between the jobs that are issued for this source file.
For example, if it is scrapped at `4 AM` then the next job will be issued for `5 AM`.

Make note that saffron will add an offset of maximum `500` seconds.

:::info
This option will override the configuration option [`scheduler.jobsInterval`](../configuration#jobsInterval)
:::

### `retryInterval`
Default value: `configuration.scheduler.jobsInterval / 2`

The interval where a source scrapping job will be reissued in case of failure.

### `timeout`
Default value: `configuration.workers.jobs.timeout`

The timeframe where Saffron will wait to get a response from an url.
If this amount is exceeded it will throw a parser error.

:::info
This option will override the configuration option [`jobs.timeout`](../configuration#jobstimeout)
:::

### `amount`
Default value: `configuration.workers.articles.amount`

The amount of articles a parser will return.

:::info
This option will override the configuration option [`articles.amount`](../configuration#articlesamount)
:::

### `ignoreCertificates`
Default value: `false`

If is set to `true` it will ignore all TLS certificates. It is useful in cases where a website
did not update their certificates.


### `extra`
This field will stay intact with whatever you put inside.
It allows the user to pass custom information about the source file.
It can be used like this:

```javascript
saffron.on('event', articles => {
    const extra = articles.getSources().extra;
    if(extra) {
        // ...
    }
})
```

### `url`
This field contains the url(s) where the news are displayed. It can be a string or an array.:

In case of one url it can be used like:

```js
url: "https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr"
//or
url: ["https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr"]
//or
url: [["https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr"]]
```

In case a website has more than one sub-sites where it displays its news, multiple urls can be used.
In that case the `scrape` options will be applied to all the urls, and it can be used like:

```js
url: [
    "https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr",
    "https://www.csd.uoc.gr/index.jsp?content=announcements&openmenu=demoAcc6&lang=gr"
]
//or
url: [
    ["https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr"],
    ["https://www.csd.uoc.gr/index.jsp?content=announcements&openmenu=demoAcc6&lang=gr"]
]
```

If you want to identify in which url an article was found you can use the categories option before the url.
It will add these categories alongside the provided url at the `categories` field of the article.

```js
url: [
    ["News", "https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr"],
    ["Annoucements", "Other category name", "https://www.csd.uoc.gr/index.jsp?content=announcements&openmenu=demoAcc6&lang=gr"]
]
```

### `type`
The type of parser that will be used during the scrapping.
For more details read about [parsers](../05_parsers/01_intro.md).

### `enconding`
The encoding of the website.

### `userAgent`
The User-Agent that will accompany the request.

### `scrape`
This field contains all the scrape options needed by the specified parser.
You can check the scrape formats for each parser: 
[WordPress](./02_wordpress.md), [RSS](./03_rss.md), [HTML](./04_html.md) or [Dynamic](./05_dynamic.md).

## Example

```json
{
  "name": "csd.uoc.gr",
  "tableName": "csd.uoc.gr",
  "interval": 3600000,
  "retryInterval": 1800000,
  "timeout": 10000,
  "amount": 10,
  "ignoreCertificates": false,
  "extra": {
      "key": "value",
      // ...
  },
  "url":  [
    "https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr",
    "https://www.csd.uoc.gr/index.jsp?content=announcements&openmenu=demoAcc6&lang=gr"
  ],
  "type": "html",
  "scrape": {
      // ...
  }
}
```
