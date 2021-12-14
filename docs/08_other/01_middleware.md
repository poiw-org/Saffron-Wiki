---
title: Middlewares
editor: markdown
sidebar_position: 1
---

A middleware function is a function that gets executed before the articles are upload to the database.

Middleware functions can be useful for:
* logging
* article formatting


## Registering a middleware

```js
saffron.use('middlewareName', (...args) => {
    //...
})
```

## Format article
For changing the contents of the articles before uploading to the database.
It gets as parameter the articles that was found from the parsers and must return the same object when it changed.

The `hash` field can also change to identify the articles based on different criteria.

```js
saffron.use("article.format", (article) => {
    // If possible set pubDate with milliseconds.
    let ms = new Date(article.pubDate).getTime()
    if(!isNaN(ms))
        article.pubDate = ms;
    
    // Delete article id so it will not be saved to the database.
    article.id = "";
    
    // Identify articles based on their link only.
    article.hash = article.link;
    
    // Return the changed article.
    return article
})
```

:::info
You can also access the source class of the article by calling `article.getSource()`. Note that any changes made on the source class will affect the parsing.
:::

## Sort articles
For sorting the articles with a specific order before uploading them to the database. It is highly recommended returning an array of articles.

```js
saffron.use("articles.sort", (articles) => {
  // ...
  return articles
})
```


## Middleware order
The order where the above extensions are executed is the order where they were called.
Each middleware function can be called more than once.
