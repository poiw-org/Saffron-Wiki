---
title: Dynamic
editor: markdown
sidebar_position: 5
---

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

