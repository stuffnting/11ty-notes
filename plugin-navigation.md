# Navigation plugin

## Simple usage

```bash
npm install @11ty/eleventy-navigation --save-dev
```

***.eleventy.js***

```js
const eleventyNavigationPlugin = require("@11ty/eleventy-navigation");

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(eleventyNavigationPlugin);
};
```

***Template files***

```yaml
Top level
---
eleventyNavigation:
  key: Mammals
---
Child
---
eleventyNavigation:
  key: Humans
  parent: Mammals
---
Title different to key
---
eleventyNavigation:
  key: Mammals
  title: All of the Mammals
---
Define an order
---
eleventyNavigation:
  key: Humans
  parent: Mammals
  order: 1
---
```

***Render***

This code is for Nunjucks, but Liquid can also be used.

```hbs
Render as HTML
{{ collections.all | eleventyNavigation | eleventyNavigationToHtml | safe }}

Render as Markdown
{{ collections.all | eleventyNavigation | eleventyNavigationToMarkdown | safe }}
```

Both `eleventyNavigationToMarkdown` and `eleventyNavigationToHtml` will render complete markup for an infinite number of levels.

## Advanced rendering

### `eleventyNavigation` filter

This returns a sorted array of menu items from a collection.

```hbs
{% set navPages = collections.all | eleventyNavigation %}
{{ navPages | dump | safe }}
```

Returns:

```json
[
  {
    "key": "Mammals",
    "url": "/mammals/",
    "title": "Mammals",
    "children": [
      {
        "key": "Humans",
        "parentKey": "Mammals",
        "url": "/humans/",
        "title": "Humans"
      },
      {
        "key": "Bats",
        "parentKey": "Mammals",
        "url": "/bats/",
        "title": "Bats"
      }
    ]
  }
]
```

To get just one branch use:

```hbs
{% set navPages = collections.all | eleventyNavigation("Mammals") %}
{{ navPages | dump | safe }}
```

### `eleventyNavigationBreadcrumb` filter

```hbs
{% set navPages = collections.all | eleventyNavigationBreadcrumb("Bats") %}
{{ navPages | dump | safe }}
```

To include the current page in the breadcrumbs, use:

```hbs
{% set navPages = collections.all | eleventyNavigationBreadcrumb("Bats", { includeSelf: true }) %}
{{ navPages | dump | safe }}
```

Allow missing pages in breadcrumbs:

```hbs
{% set navPages = collections.all | eleventyNavigationBreadcrumb("Does not exist", { allowMissing: true }) %}
{{ navPages | dump | safe }}
```

### Using `eleventyNavigationToHtml` with options

The full set of options is:

```js
---js
{
  navigationOptions: {
    listElement: "ul",            // Change the top level tag
    listItemElement: "li",        // Change the item tag

    listClass: "",                // Add a class to the top level
    listItemClass: "",            // Add a class to every item
    listItemHasChildrenClass: "", // Add a class if the item has children
    activeListItemClass: "",      // Add a class to the current page’s item

    anchorClass: "",              // Add a class to the anchor
    activeAnchorClass: "",        // Add a class to the current page’s anchor

    // If matched, `activeListItemClass` and `activeAnchorClass` will be added
    activeKey: "",
    // It’s likely you want to pass in `eleventyNavigation.key` here, e.g.:
    // activeKey: eleventyNavigation.key

    // Show excerpts (if they exist in data, read more above)
    showExcerpt: false
  }
}
---
```

**Example: add excerpts**

```yaml
---
title: Humans
eleventyNavigation:
  key: PooMans
  parent: Mammals
  excerpt: Vertebrate animals of the class Mammalia.
---
```

```hbs
---
navToHtmlOptions:
  showExcerpt: true
---
{{ collections.all | eleventyNavigation("Humans") | eleventyNavigationToHtml(navToHtmlOptions) | safe }}
```

This renders the title and link, as before, followed by the excerpt text.

### Render with custom HTML

This code uses custom HTML to render a nav menu with a single level, without using `eleventyNavigationToHtml`.

```hbs
{% set navPages = collections.all | eleventyNavigation %}
<ul>
{%- for entry in navPages %}
  <li{% if entry.url == page.url %} class="my-active-class"{% endif %}>
    <a href="{{ entry.url }}">{{ entry.title }}</a>
  </li>
{%- endfor %}
</ul>
```

### An example using the `eleventyComputed` object

This method can be used to setup navigation data for a whole directory in a single file, rather tha the font matter of each individual template file.

If there are page templates contained in the folder `/animals/`, the `eleventyComputed` object can be set in the directory data file `/animals/animals.11tydata.js` file. If you wanted to compute this data for all template files, it could be computed in a global data file.

***/posts/posts.11tydata.js***

```js
module.exports = {
  eleventyComputed: {
    eleventyNavigation: {
      key: (data) => data.title,
      parent: (data) => data.parent,
    },
  },
};
```

Note, since template data files have a higher priority than front matter data, `data.title` and `data.parent` are available form the template files' front matter.

```yaml
___
title: Humans
parent: Mammals
___
```

The data generated from the template file is now:

```js
{
  "title": "Humans",
  "parent": "Mammals",
  "eleventyNavigation": {
    "key": "Humans",
    "parent": "Mammals"
  }
}
```
