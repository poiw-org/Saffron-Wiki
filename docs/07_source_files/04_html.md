---
title: HTML
editor: markdown
sidebar_position: 4
---

## HTML parser
Below is an example of the HTML parser source file:
```json
{
  "type": "html",
  // ...
  "scrape": {
    "container": ".classNameExample",
    "endpoint": "example.org",
    "article": {
      	// information to pull from article.
      	...
     }
   }
}
```
* `container`: Refers to the HTML class in which all the articles are located. Usually we search with the **inspect element tool** of the browser for the class that covers all the content we want.
* `endpoint`: The domain of the website. It is used complete the article links.
* `article`: This field contains all the information about what the html parser should scrape from the desired website. In order to receive this information, each piece of data must be identified separately.

For each value we want to take from an article we write it as follows
```json
  "value": { }
```
For resolving the title, contents, publish date or attachments of the article, replace `<value>` with:
* `title` for the title of the article.
* `content` for the contents of the article.
* `pubDate` for the publish date of the article.
* `attachments` for the attachments of the article.
* `link` for the link of the article.

Any other information will be saved at the `extras` field of the article.

Each value is placed somewhere in the website, so we will need some information to find them:

```json
  "value": {
    "class": "my_container"                         // string
    "attributes": [ "href", "src", "value", ... ],  // array
		"find": [ ".class1", "li", "#id" ],             // array
		"multiple": true | false                        // boolean
  }
```

`class`: this value represents the html class where the title is displayed.

`attributes`: a string array that contains `html` attributes that are going to used to scrape some specific information like links or images. Some popular attributes are:
* `href` to get the link from a view.
* `src` to get the link from an image.
* `value` to get the actual value that is contained in a view `<a>my_value</a>`.

Notice, if the attributes are not found, then they will be empty.

`find`: in case the actual element cannot be found by class alone, we use the find option. A string array that contains multiple classes, ids or tags in the right order. It will describe the path to reach the desired value. It will search for the value starting from the inside of the `class` option above. Notice, classes must start with `.` and ids with `#`.

`mutliple`: in case there are multiple links, images or other attributes in the desired article, enable this option so it can scrape them all.

An example using one greek university `UniPi`:
```json
{
  "url": "https://www.unipi.gr/unipi/el/%CE%B1%CE%BD%CE%B1%CE%BA%CE%BF%CE%B9%CE%BD%CF%8E%CF%83%CE%B5%CE%B9%CF%82.html",
  "name": "Univeristy of Piraeus",
  "type": "html",
  "retryInterval": 1800 * 1000, // 0.5 hours
  "scrapeInterval": 3600 * 1000, // 1 hour
  // Scraping options
  "scrape": {
    "container": ".catItemView",
    "endPoint": "unipi.gr",
    "article": {
      // Get the title of the article
      "title": { 
        "class": ".catItemTitle",
        "find": [ "a" ],
        "multiple": false
      },
      // Get the content of the article
      "content": {
        "class": ".catItemBody",
        "find": null,
        "multiple": false
      },
      // Get the publish date of the article
      "pubDate": {
        "class": ".catItemDateCreated",
        "attributes": [ "value", "href" ],
        "find": null,
        "multiple": false
      }, 
      // Get the embedded links of the article
      "attachments": {
        "class": ".catItemLinks",
        "attributes": [ "value", "href" ],
        "find": [ ".catItemAttachmentsBlock", "li", "a" ], // carefull on the order
        "multiple": true
      }
    }
  }
}
```