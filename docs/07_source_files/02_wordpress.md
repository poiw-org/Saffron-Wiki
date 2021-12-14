---
title: WordPress
editor: markdown
sidebar_position: 2
---


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
