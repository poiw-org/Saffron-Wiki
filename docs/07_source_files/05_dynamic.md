---
title: Dynamic
editor: markdown
sidebar_position: 5
---

## Template

Unlike the other parse this parser uses javascript files, so the options described [here](./01_general.md) must be included
inside the javascript file. Also, the dynamic parser uses a scrape function instead of scrape options.

Below is a template for the dynamic parser source file:
```js
const {types: {Article, Utils}} = require("@poiw/saffron")
const utils = new Utils();

module.exports = {
    name: "csd.uoc.gr",
    type: "dynamic",
    // ...
    scrape: async function () {
        // Do your thing...
    }
}
```

## Starting

First you have to import the `types` needed for generating an article and initialize the utils needed during the parsing.
Then you can create a JavaScript object containing all the necessary fields for the source file to function.

Second you create the `scrape` asynchronous function to write down code needed for scraping.

:::info
Any extra libraries used here must be added to your `package.json` file.
:::

## Utils

Utils provide a set of necessary functions and fields that will be useful during scrapping.

### `isFirstScrape`
Type: `field:boolean`

This field will be set to true if there are no other articles in the database.

### `isScrapeAfterError`
Type: `field:boolean`

This field will be set to true if the previous source scrapping job has failed.

### `getArticles`
Type: `function`

This function will return a specified amount of articles that belong to the same source.
The amount can be specified using the parameter `amount` and it can return up to 50 articles.
If `amount` is a negative number, an exception will be thrown.

:::info
New articles added from [`onNewArticle`](#onnewarticle) will be pushed at the start of the array.
:::

### `onNewArticle`
Type: `function`

A function that enables you to store a new article. It takes as parameter the article that you want to save.
If an error occurs during the scrapping, no articles will be saved at the database.

### `url`
Type: `field:string`

This field contains the currently working url. There is no need to create support for multiple urls because we do it for you.

### `parse`
Type: `function:Article[]`

Dynamic parser can utilize the functionality of the other parsers, by using the `parse` function and passing as
parameter a source file's contents. In case of incorrect source file or failed parsing an `Exception` will be thrown.


## Writing code

### Nested functions

The dynamic parser can only see the `scrape` function and not other functions.
If you want to create separate functions you have to utilize them inside the `scrape` function.

```js
scrape: async () => {
  const log = (message) => {
    console.log(message);  
  }
  
  log('This is the nested function.');
}
```

### Fail job

In case you want to mark the scrapping a failed and return no articles then you have to throw an `Error`.

```js
scrape: async () => {
    // ...
    throw new Error("Parsing failed.");
}
```
