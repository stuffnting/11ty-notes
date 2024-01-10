
# Collections

Collections are pieces of content that are grouped together using `tags` in their front matter. A piece of content can be in multiple collections.

## Adding tags

**A single tag**

```yaml
---
tags: post
title: Word to the wise: a blog post
---
```

**Multiple tags as a string**

```yaml
---
tags: cats and dogs
---

**Multiple tags as an array**

```yaml
---
tags: ["cat", "dog"]
---
```

**Multiple tags on multiple lines**

```yaml
---
tags:
  - cat
  - dog
---
```

## Simple blog setup

Front matter can be set for a whole folder using a JSON file.

The post HTML files are automatically generated from the `posts` folder.

```
.
├── _includes
│   └── layout.html
├── _site
│   ├── README
│   │   └── index.html
│   ├── about
│   │   └── index.html
│   ├── index.html
│   └── posts
│       ├── post-1
│       │   └── index.html
│       └── post-2
│           └── index.html
├── index.html
└── posts
    ├── post-1.md
    ├── post-2.md
    └── posts.json

```

The `posts.json` file specifies how the MD files in the `posts` directory should be handled. 11ty, by default, looks in the `_includes` folder for template files (the folder name can be changed in the 11ty config file).

```json
{
  "layout": "layout.html",
  "tags": "post"
}
```

The `tags` is used to specify what 11ty calls a "collection", which is use in the blog index file, that lists the posts.

```hbs
{% for post in collections.post %}

<h2><a href="{{post.url}}">{{post.data.title}}</a></h2>

<p>{{post.content}}</p>

{% endfor %}
```

## The `all` collection

All content pages placed within the `collections.all` collection, whether tags were assigned to it or not.

## `eleventyExcludeFromCollections`

This can be used to exclude content from all collections, even when `tags` are set.

```yaml
---
eleventyExcludeFromCollections: true
tags: post
---
This will not be available in `collections.all` or `collections.post`.
```

## Custom collections and sorting methods

### Default sorting - created date

The default sorting is by the files Created Date. Files have the same Created Date, their full paths are used to sort them. See here for [more](https://www.11ty.dev/docs/collections/#sorting).

### Override the Created Date

This is achieved using `date` in the content's front matter.

```yaml
---
date: 2016-01-01
---
```

### Sort descending in template files

This can be done with the Nunjucks `reverse` filter ([other templating options](https://www.11ty.dev/docs/collections/#sort-descending)).

```hbs
<ul>
  {%- for post in collections.post | reverse -%}
  <li>{{post.data.title}}</li>
  {%- endfor -%}
</ul>
```

Or, set a different file related date (see [here](https://www.11ty.dev/docs/dates/#setting-a-content-date-in-front-matter) for the full list):

```yaml
date: Last Modified
```

### Custom collections and sorting methods

You can create your own sorting filters in the `.eleventy.js` config file.

The filters use the following pattern:

```js
module.exports = function (eleventyConfig) {
  // Unsorted items (in whatever order they were added)
  eleventyConfig.addCollection("allMyContent", function (collectionApi) {
    return collectionApi.getAll();
  });
};
```

There are lost of examples of filters [here](https://www.11ty.dev/docs/collections/#advanced-custom-filtering-and-sorting), including ignoring `tags` and filtering all content via specified rules, getting content that is listed with defined multiple `tags`, only returning content that has come from MD files, use globs to select files etc.

#### Example of a custom collection

This example defines a collection that includes all of the MD files from the `/posts`, and `/more-templates` folders.

***`.eleventy.js`***

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addCollection("moreStuff", function (collectionApi) {
    return collectionApi.getFilteredByGlob([
      "more-templates/*.md",
      "posts/*.md",
    ]);
  });
};
```

***Template file***

```hbs
{% for post in collections.moreStuff %}

<h2><a href="{{ post.url }}">{{ post.data.title }}</a></h2>

<p>{{ post.content | safe }}</p>

{% endfor %}
```

#### Example of a custom collection with a sort method

***News item template file***

```yaml
---
title: Update March 2006
order: 0
permalink: "up03_06.htm"
---
```

***`.eleventy.js`***

```js
module.exports = function (eleventyConfig) {

  eleventyConfig.addCollection('newsOrdered', function (collectionApi) {
    return collectionApi.getFilteredByTags('news').sort(function (a, b) {
      console.log(a.data.order);
      return b.data.order - a.data.order; // sort by order - descending
    });
  });
  
};
```

***News archive template file***

```hbs
<ul  class="archive-list">

{% for newsPost in collections.newsOrdered %}

<li><a href="{{newsPost.url}}">{{newsPost.data.title}}</a></li>

{% endfor %}

</ul>
```

## Collection data structure

Each item in a collection looks like:

```js
{
  template: Template {
    ...
  },
  data: {
    ...
  },
  page: {
    ...
  },
  ...
}
```

Leaving out the `template` property, and expanding the others:

```js
data: {
  eleventy: { version: '2.0.1', generator: 'Eleventy v2.0.1', env: [Object] },
  pkg: {
    ...
  },
  layout: 'layout.html',
  tags: [ 'post' ],
  title: 'My post 1',
  page: {
    ...
  },
  collections: { all: [Array], post: [Array] }
},
page: {
  date: 2023-07-08T12:10:44.447Z,
  inputPath: './posts/post-1.md',
  fileSlug: 'post-1',
  filePathStem: '/posts/post-1',
  outputFileExtension: 'html',
  templateSyntax: 'liquid,md',
  url: '/posts/post-1/',
  outputPath: '_site/posts/post-1/index.html'
},
inputPath: './posts/post-1.md',
fileSlug: 'post-1',
filePathStem: '/posts/post-1',
date: 2023-07-08T12:10:44.447Z,
outputPath: '_site/posts/post-1/index.html',
url: '/posts/post-1/',
templateContent: [Getter/Setter],
content: [Getter]
```

***Notes***

- `page`: everything in Eleventy’s supplied `page` variable for this template (including `inputPath`, `url`, `date`, and `others`). **Use these values n preference to the top-level values.** (Added in v2.0.0.)
- `data`: all `data` for this piece of content (includes any `data` inherited from layouts, template specific, layout specific, and global data). Note, the `data` keys are available in collection templates as variables, e.g. {{ `date` }}, {{ `title` }} etc., but when creating archive pages via `pagination`, they are accessible through `item.data.title` etc. (where `item` is the iteration variable)
- `content`: the rendered `content` of this template. This does not include layout wrappers. (Added in v2.0.0)

Backwards compatibility:

- Top level properties for ` inputPath`,  `fileSlug`, `outputPath`, `url`, `date`are still available, though use of`page.\*` (Added in v2.0.0) for these is encouraged moving forward.
- `content` (Added in v2.0.0) is aliased to the previous property `templateContent`.
