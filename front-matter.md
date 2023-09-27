
# Front matter

[YAML](https://learnxinyminutes.com/docs/yaml/) is the default for front matter, however, other formats can be used; even custom formats are possible. See [here](https://www.11ty.dev/docs/data-frontmatter/#alternative-front-matter-formats) for more.

The items in the front matter are made available to the layout file.

There are some default front matter keys, called special keys. These key names are reserved, and can't be used for custom data in the data cascade. See [here](https://www.11ty.dev/docs/data-configuration/).

## Basic use

This is a sample of basic use, however, there are lots ways to add data (see [here](https://learnxinyminutes.com/docs/yaml/)).

```yaml
---
layout: main-layout.njk
title: A chained layout file
myObjectFM:
  size: XL
  style: Whitey Tighty
myArrayFM:
  - XL
  - Whitey Tighty
---
```

In the date section of a collection item: `myObjectFM` will be an object, with key/value pairs; and, `myArrayFM` will be an array.

## Front matter formats

Eleventy uses [the gray-matter package](https://github.com/jonschlinkert/gray-matter) for front matter processing. gray-matter (and thus, Eleventy) includes support out of the box for YAML, JSON, and even JavaScript object literals in front matter.

**YAML**

```yaml
---
title: My page title
---
```

**JSON**

```json
---json
{
  "title": "My page title"
}
---
```

**JS object literal**

This can incorporate JS functions to be used in the template/layout.

```js
---js
{
  title: "My page title",
  currentDate: function() {
    // You can have a JavaScript function here!
    return (new Date()).toLocaleString();
  }
}
---

<h1>{{ title }}</h1>
<p>Published on {{ currentDate() }}</p>
```

## Custom front matter formats

It is possible use front matter in other forms, which are supported by grey-matter, but not 11ty by default.

For example, use TOML:

***.eleventy.js***

```js
const toml = require("@iarna/toml");

module.exports = function (eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    engines: {
      toml: toml.parse.bind(toml),
    },
  });
};
```

It is also possible to write the front matter in JS, rather than a JS object literal. See [here](https://www.11ty.dev/docs/data-frontmatter-customize/) for more details.

## Template strings in YAML front matter

The `permalink` special key allows for template strings to be used in YAML front matter.

```yaml
permalink: "/gin-reviews/{{ gin.slug }}/"
```

The only other way of using template strings in YAML front matter is to use the `eleventyComputed` special data key. This allows for any data item to be set, or modified, using template strings.

```yaml
eleventyComputed:
  title: "{{gin.name }}"
```

In this example, setting `title` directly, without using `eleventyComputed` will not work.

## Bulk set front matter

Front matter can be set for a whole folder using a JSON file. See the blog example [below](#simple-blog-setup).
