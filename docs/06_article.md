---
title: Article
editor: markdown
sidebar_position: 6
---


We have created a universal format for the parsed news that exit the saffron and stored to the database, and we named `Article`.
The article itself is in `JSON` format.

## Custom data

### `id`
A random field assigned by saffron to identify the article.
All article ids starts with the prefix `art`.

### `timestamp`
The date in milliseconds where the article was scrapped.

### `source`
An object containing the source's id and name where the article belongs to.

## Article data

### `title`
This field is the title of the article.

### `content`
This field is the content of the article. It may contain html tags based on the website.

### `link`
This field is the url of the article. It is supported to redirect the user the article's content
in the website that was scrapped from.

### `pubDate`
This field is the publication date that was retrieved from the website.

### `attachments`
This field will contain any kind of attachments that was found with the article.
It will also contain the extracted urls that exists inside the `content`.

### `categories`
The categories where the article belongs to. The categories assigned at the source file will also
be added here.

### `thumbnail`
The thumbnail's url if there is any.

### `extras`
Extra information about the article that did not belong to the above fields.
