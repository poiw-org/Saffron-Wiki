---
title: Parsers
published: true
date: 2021-07-29T20:54:08.892Z
editor: markdown
dateCreated: 2021-07-19T05:00:11.568Z
sidebar_position: 3
---

# Parsers

To retrive the desired information from the websites we use unique parsers. Right now we have four different parser types, `Html`, `Rss`, `Dynamic` and `Wordpress`.

The `HTML` parser works by downloading the raw HTML version of a given website and iterating over the children elements as stated in the source file configuration. ***Beware that in websites that use a WAF and employ bot-blocking techniques such as a Javascript challenge, the HTML parser may not work correctly!***
The `RSS` parser uses a website's RSS feed to parse its articles. Notice that for this parser to work, the website administrator has to enable or implement its RSS feed.
The `WordPress` parser is a case-specific parser that can be used on any website using Wordpress. The parser will use the `wp-json` endpoints to parse articles in a clean and stable way.
The `Dynamic` parser gives you unlimited options to develop your own custom parser. In a `Dynamic` parser, you can utilize the `npm` modules of choice and your own scraping logic. The only thing you will need to do is return each time an `Array` of `Article` instances.

How to decide which scrapper to use? The is an order.
* If the website supports `WordPress API` then use the `WordPress` parser.
* If the `RSS` feed is available, then use the `RSS` parser.
* If the webiste has a structured form, then use the `HTML` parser.
* Last choice is if none of the above are possible, use the `Dynamic` parser.


This page is maintained by: [JexSrs](https://github.com/JexSrs)