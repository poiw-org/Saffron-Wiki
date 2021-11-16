---
title: Source files
published: true
date: 2021-11-15T19:02:34.508Z 
editor: markdown
dateCreated: 2021-07-19T05:05:06.142Z
sidebar_position: 4
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
  "scrape": {}
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
* `scrape`: Contains all the scrape options for the parsers.

Each parser has its own scrape format.



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


## RSS parser



## WordPress parser
Below is an example of the WordPress parser source file:
```json
{
  "url": "example.com",
  "name": "Example site",
  "type": "wordpress"
}
```
* `url`: Above we said that this field is the url where the articles are displayed. Because the WordPress CMS offers a default API for the articles, here we need the base URL of the website.

To check if the desired website support the WordPress API just open your browser with the base url and append this subUrl `/wp-json/wp/v2/posts/`. For example: `www.ds.unipi.gr/wp-json/wp/v2/posts/`. If the browser returns a JSON array of posts, then the url is valid.



## Dynamic parser
Below is an example of the dynamic parser source file:
```js
const {types: {Article, Utils}} = require("../../dist/index")
const utils = new Utils()

module.exports = {
    type: "dynamic",
  	...
    scrape: async () => { }
}

```

Unlike the other, the dynamic parser make use of a `javascript` source file instead of json.
* `scrape` is an asynchronous function that will be called from the parser.

* `Article` is a javascript class that will be used to store each article's information.

The constructor accepts two parameters:
* A string type message, that describes the error
* A boolean that can lock the source file. If a source file is locked it will not be parsed until 		it is manually unlocked.

* `utils` is a sum of functions and variables that will be used during the scrapping proccess.
    * `isFirstScrape` a boolean type variable that returns if it the first time the source file is scrapped.
    * `isScrapeAfterError` a boolean type variable that return if the last time the scrape returned 		 an `Exceptions` class.
    * `getArticles(count)` a function that will return an array of the last `<count>` parsed 			        articles.
    * `onNewArticle(article)` a function that will be used to add a new article to the stack.
    * `url` a string type variable which containes the url that is passed from the source file.

Frequently asked Questions:
* **Can I create more functions outside of the already declared `scrape` function?**
  You can't, the parser will only read the `scrape` function. On the other hand you can create private functions inside the scrape function:
```js
const secondFunc = () => { ... }
```
* **After I call the `onNewArticle` function will be the article available at the `getArticles` function?**
  Yes, every time you call `onNewArticle` function the passed article is temporary saved and appended at the top of the already parsed articles. When the scraping is finished all the scraped articles will be added to the desired database.



See also: [Article contents](https://saffron.poiw.org/en/article), [Source file extension](https://github.com/UniStudents/saffron-file-generator), [Testing the source files](https://saffron.poiw.org/)
