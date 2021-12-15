---
title: General
editor: markdown
sidebar_position: 1
---

## What is a source file?

A source file is a `json` or `javascript` file. These files are generated from the user and help Saffron to parse news from the desired websites.
Therefor each source file represents a website.
Each parser utilizes a different source file structure but there are some common options.

## Options

### `name`
Default value: none

This option identifies the source file. It is also used in configuration [`includeOnly`](../configuration#includeonly)
and [`exclude`](../configuration#exclude). It must be unique for each source file.

### `tableName`
Default value: none (optional)

Saffron implements a variety of database drivers. Either it is SQL or not. This option allows the database to know in which
table (sql) or collection (no-sql) to store the articles. If it is not specified, the `name` will be used in its place.

### `interval`
Default value: `3600000` (optional)

The interval where a source scrapping job will be generated for the source file.

:::info
This option will override the configuration option [`jobsInterval`](../configuration#jobsInterval)
:::

### `retryInterval`
Default value: `jobsInterval / 2` (optional)

The interval where a source scrapping job will be reissued in case of failure. If it not set it will take
the configuration's option [`jobsInterval`](../configuration#jobsInterval) and divide it by two.

### `timeout`
Default value: `10000` (optional)

The timeframe where Saffron will wait to get a response from an url. If this amount is exceeded it will throw a parser error.

:::info
This option will override the configuration option [`jobs.timeout`](../configuration#jobstimeout)
:::

### `amount`
Default value: `10` (optional)

The amount of articles will parse from an url.

:::info
This option will override the configuration option [`articles.amount`](../configuration#articlesamount)
:::

### `ignoreCertificates`
Default value: `false` (optional)

If is set to `true` it will ignore expired TLS certificates. In that case the program will not be protected
against man in the middle attacks.


### `extra`
Default value: none (optional)

This field was created mainly for analytic purposes. It allows the user to pass any kind of information inside that field.
The field can then be accessed later through an article instance using the call `article.getSource().extra`


### `url`
Default value: none

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

If you want to identify in which url an article was found you can use the category option before the url.
It will add that category alongside the provided url at the `categories` field of the article.

```js
url: [
    ["News", "https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr"],
    ["Annoucements", "https://www.csd.uoc.gr/index.jsp?content=announcements&openmenu=demoAcc6&lang=gr"]
]
```

### `type`
Default value: none

The type of parser that will be used during the scrapping. For more details read about [parsers](../05_parsers/01_intro.md).


### `scrape`
Default value: none

This field contains all the scrape options needed by the specified parser. You can check the options needed for each parser
by clicking the parser you want to use: [WordPress](./02_wordpress.md), [RSS](./03_rss.md), [HTML](./04_html.md) or [Dynamic](./05_dynamic.md).

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
  "extra": {},
  "url":  [
    "https://www.csd.uoc.gr/CSD/index.jsp?content=news&openmenu=demoAcc6&lang=gr",
    "https://www.csd.uoc.gr/index.jsp?content=announcements&openmenu=demoAcc6&lang=gr"
  ],
  "type": "html",
  "scrape": {}
}
```
