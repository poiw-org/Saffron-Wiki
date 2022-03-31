---
title: Introduction
editor: markdown
sidebar_position: 1
---


To retrieve the desired information from the websites we use parsers.
There are four available parser types: `wordpress`, `rss`, `html` and `dynamic`.

## WordPress V2
Parser type: `wordpress-v2`

By default, [`WordPress`](https://wordpress.com/) based websites has an open API for news retrieval.
We make use of that to get access on the posts and categories of the website.

To check if a website supports that API simply open your browser and type `<website-root-link>/wp-json/wp/v2/posts/`.
If a valid JSON file is displayed on the browser (or downloaded on your computer) which contains the website's posts,
then you can safely use the `wordpress` parser.


## RSS
Parser type: `rss`

Many websites support [`RSS`](https://en.wikipedia.org/wiki/RSS) feed. RSS allows users and applications to access updates
to websites in a standardized, computer-readable format. You can check if a website supports RSS if you can see this
icon <img src="/img/rss.png" width="20" height="20" />.


## HTML
Parser type: `html`

This parser uses scrapping tools like [CheerioJS](https://cheerio.js.org/) to scrape the website content and receive
the displayed news. This parser is best to be used when the HTML in the website is structured. Websites where the HTML
and CSS are not structured will be very difficult to scrape.

## Dynamic
Parser type: `dynamic`

Unlike the other parsers, this parser uses javascript code to parse a website. All the logic for the scraping is
decided by the user.