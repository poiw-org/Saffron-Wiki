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
module.exports = {
    name: "csd.uoc.gr",
    type: "dynamic",
    // ...
    scrape: async function (utils, Article) {
        // Do your thing...
        
        const article = new Article();
        article.title = '';
        // ...
        
        if(error)
            throw new Error('Failed for csd.uoc.gr!');
        
        return articles;
    }
}
```

## Starting

Create the `scrape` asynchronous function to write down code needed for scraping.
Saffron will ignore the rest of the file when it comes to scrapping so
all code must be included inside the scrape function, such as imports user-defined functions etc.

:::info
Any extra libraries used here must be added to your `package.json` file.
:::

## Utils

Utils provide a set of necessary functions and fields that are used by all scrappers
and will be needed for you to.

### `isScrapeAfterError`
A field that will return `true` if the previous job was a failure.

### `url`
A field that contains the url that must be scrapped.

### `aliases`
A field that contains the categories passed alongside the url.
You do not have to insert them to each article, saffron is doing
this for you.

### `instructions`
A set of instructions. It is mainly used from other parsers and
also contains the current scrape function at the `scrapeFunction` field as a string.

### `amount`
The maximum amount of articles that has to be returned.

### `get`
A middleware function for axios to do a `GET` request.

### `post`
A middleware function for axios to do a `POST` request.

### `request`
A middleware function for axios to do any kind of request.

### `parse`
A function that will get as its first parameter the contents
of another source file and return the articles that was scrapped.

In case of incorrect source file or failed parsing an `Exception` will be thrown.

This function will return an object array that contains the
url, aliases and articles for each url specified at the
nested source file.

### `htmlStrip`
A function that will strip a string of all html tags

### 'extractLinks'
A function that gets as its first parameter valid HTML code
and returns an attachment array of the urls of the following tags:
`a`, `img`, `link`.

## Writing code

### Nested functions & Imports
Saffron will ignore the rest of the file when it comes to scrapping so
all code must be included inside the scrape function, such as imports user-defined functions etc.

```js
scrape: async (utils, Article) => {
  const log = (message) => {
    console.log(message);  
  }
  
  const other = require('other');
  log('Using a nested function.');
  // ...
}
```

### Callbacks
Dynamic parser does not utilize callbacks. If you want to use callbacks in your code you
have to return a promise:

```javascript
scrape: (utils, Article) => {
  return new Promise((resolve, reject) => {
      utils.get(utils.url).then(response => {
          // ...
          
          resolve(articles);
      });
  });
}
```

### Fail job

In case you want to mark the scrapping as failed and return no articles then you have to throw an `Error`:

```js
scrape: async (utils, Article) => {
    // ...
    throw new Error("Parsing failed.");
}
```
or reject the promise:
```js
scrape: async (utils, Article) => {
    return new Promise((resolve, reject) => {
        // ...
        reject(new Error("Parsing failed."));
    });
}
```
