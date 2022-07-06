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
It gets as parameter every article that was found from the parsers and must return the same object when it changed.

```javascript
saffron.use("article.format", (article) => {
    // If possible set pubDate with milliseconds.
    let ms = new Date(article.pubDate).getTime()
    if(!isNaN(ms))
        article.pubDate = ms;
    
    // Delete article id so it will not be saved to the database.
    article.id = "";
    
    // Return the changed article.
    return article
})
```

If you want to customize the website's article identification override the `getHash` function.
```javascript
// Will identify an article based on its link and title.
article.getHash = () => `${article.link}:${article.title}`;
```

:::info
You can also access the source class of the article by calling `article.getSource()`.
Note that any changes made on the source class will also affect the saved source.
:::

## Articles
This middleware can be used to edit the articles in bulk. You can sort or filter them as you want.
The only requirement is to return an array (empty or not) of articles.

```js
saffron.use("articles", (articles) => {
  // ...
  return articles
})
```

## Middleware order
The order where the above extensions are executed is the order where they were called.
Each middleware function can be called more than once.
