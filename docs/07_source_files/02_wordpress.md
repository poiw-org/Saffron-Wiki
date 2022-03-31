---
title: WordPress
editor: markdown
sidebar_position: 2
---

## WordPress parser

Below is an example of the WordPress parser source file:

```json
{
    "type": "wordpress-v2",
    // ...
    "scrape": {
        "articles": {
            "include": ["id", "modified"],
            "dates": {
                "gmt": false,
                "fallback": false
            },
            "filter": {},
            "thumbnail": "thumbnail"
        }
    }
}
```

About `articles` children:
* `include`: Is an array of properties that will be appended to the final article extras field.
        The properties that are allowed, are all placed at the root of each post that comes
        from the `WordPress` JSON response.
        For example the values `id` and `modified` are not saved by default, but can be
        saved by including them to the array. All invalid objects will be skipped.
* `dates.gmt`: If `true` it will replace the [`puDate`](../06_article.md#pubdate) with GMT
        timezone, if not it will take the site's timezone.
* `dates.fallback`: If `true` and GMT format is not found it will fall back to the site's timezone.

* `filter`: Filters the requested articles using the filters given by the WorPress REST API.
  * `search`: with string value.
  * `author`: with string value of integers, split with comma.
  * `authorExclude`: with string value of integers split with comma.
  * `after`: with string value of ISO8601 date format.
  * `before`: with string value of ISO8601 date format.
  * `slug`: with string value.
  * `categories`: with string value of integers, split with comma.
  * `categoriesExclude`: with string value of integers, split with comma.
  * `tags`: with string value, split with comma.
  * `tagsExclude`: with string value, split with comma.
  * `sticky`: with boolean value.
* `thumbnail`: Takes as value the size of the main image, if the post has one and returns the image's source url.
    The available sizes can be found under the `_embedded` property.
    Full path is: `['_embedded']['wp:featuredmedia'][0]['media_details']['sizes']`
