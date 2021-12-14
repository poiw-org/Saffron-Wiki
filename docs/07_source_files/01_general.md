---
title: General
editor: markdown
sidebar_position: 1
---

# Source Files

`Saffron` can parse any desired website as long as it has the appropriate source file. The source files by default are placed at the project root directory in the `sources/` folder. If the source files are placed in a different folder, then pass down to source folder `path` to configuration. The source files file format is `.json`.

If a source file fails to meet the below criteria, it will be skipped and log an install error.

Each source file need some basic information regardless the selected parser.
```json
{
  "name": "example",
  "collection_name": "",
  "url": "https://example.com",
  "ignoreCertificates": false,
  "type": "html",
  "scrapeInterval": 3600
  "retryInterval": 1800,
  "requestTimeout": 5000,
  "extra: "".
  "scrape": {},
}
```
* `name`: The name of the source file. Warning, this field is used for connecting the articles with the website, changing it, is the same as discarding all the old articles. Also assigning two source files the same name is prohibited and will cause the scheduler to malufanction.
* `collection_name`: The collection name where the articles of this source files will be stored. If it is not defined it will take the `name` field.
* `url`: The url where the articles are displayed. In case there are multiple urls for the same scrape format, change the url to an array containing arrays of `alias` / `url`.
  ```json
	"url": [
    ["alias1", "url1"],
    ["alias2", "url2"],
    ...
  ]
  ```
  The `alias` must be at position zero (0) and the `url` at position one (1). Neither of them cannot be empty. The `alias` is stored in the `extras` field of the article. The `wordpress` parser does not support multiple urls.
  See also: [Article contents](https://saffron.poiw.org/en/article)
* `ignoreCertificates`: If a website has expired certificate and it is set to false, then it will throw parsing error.
* `type`: Can be "html", "rss", "wordpress" and "custom". This field decides which parser to use.
* `scrapeInterval`: The interval in milliseconds between the scraping jobs.
* `retryInterval`: In case the first scraping fails, when to try again (in milliseconds).
* `requestTimeout`: The request timeout for the specific source
* `extra`: Put extra data not related to saffron, can be any type, string, object, arrays etc.
* `scrape`: Contains all the scrape options for the parsers.

Each parser has its own scrape format.
