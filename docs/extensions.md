---
title: Extensions
published: true
date: 2021-09-30T11:34:27.163Z
editor: markdown
dateCreated: 2021-08-09T10:52:52.469Z
sidebar_position: 6
---

# Extensions

Saffron supports extension function for some extra functionalities.

### Format article
For changing the contents of the articles before uploading them to the database.
```js
saffron.use("article.format", (article) => {
  // ...
  return article
})
```
It is required to return an instance of an article, if not an exception will be throwed.
You can access the source class of the article by calling `article.getSource()`

Notice this function is called after `workers.articles.new` event.



### Sort articles
For sorting the articles with a specific order before uploading them to the database.
```js
saffron.use("articles.sort", (articles) => {
  // ...
  return articles
})
```
It is required to return an array of object that are instance of an article, otherwise an exception will be throwed.



### Article hash
It possible to change the hash of the articles, meaning customizing the condition where the articles are compared to each other.
```js
saffron.use("articles.hash", (article) => {
  // ...
  return article.hash
})
```
It is required to return a string that contains the new hash of the given article, if not an exception will be throwed.



### Extension order
The order where the above extensions are executed is the following:
1) `article.format`
2) `articles.sort`
3) `article.hash`
4) Database upload


This page is maintained by: [JexSrs](https://github.com/JexSrs)