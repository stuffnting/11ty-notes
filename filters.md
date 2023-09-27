# Filters

Filters can be used by JavaScript , Markdown, Nunjucks, Liquid, Handlebars, and WebC template. Markdown files are preprocessed as Liquid templates (by default, [the template engine can be changed](https://www.11ty.dev/docs/config/#default-template-engine-for-markdown-files)), and therefore filters can be used in Markdown files too.

## Built-in filters

**[`url`](https://www.11ty.dev/docs/filters/url/):** Normalize absolute paths in your content, allows easily changing deploy subdirectories for your project.

**[`slugify`](https://www.11ty.dev/docs/filters/slugify/):** "My string" to "my-string" for permalinks.

**[`log`](https://www.11ty.dev/docs/filters/log/):** console.log inside templates. Can be chained.

**[`get*CollectionItem`](https://www.11ty.dev/docs/filters/collection-items/):** Get next or previous collection items for easy linking using: `getPreviousCollectionItem` and `getNextCollectionItem`

## Add filters

Custom filters can be added in the config file.

```js
module.exports = function(eleventyConfig) {
  eleventyConfig.addFilter("myFunctionName", function(value) { … });
};
```

Filters can also be added for individual template engines, using:

**[`addJavaScriptFunction`](../11ty.dev/docs/languages/javascript#filters)**

**[`addLiquidFilter`](https://www.11ty.dev/docs/languages/liquid/#filters)**

**[`addNunjucksFilter`](https://www.11ty.dev/docs/languages/nunjucks/#filters)**

**[`addHandlebarsHelper`](https://www.11ty.dev/docs/languages/handlebars/#filters)**

For example:

```js
module.exports = function(eleventyConfig) {
  // Nunjucks Filter
  eleventyConfig.addNunjucksFilter("myNjkFilter", function(value) { … });
};
```

## Using filters in template (Nunjucks)

The first argument of the filter callback function is the value being filtered, subsequent parameters can be passed with the function call.

***eleventy.js***

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addFilter("allCaps", function (value, extra) {
    return value.toUpperCase() + extra;
  });
};
```

***my-template.nks***

```hbs
<p>{{ "pants" | allCaps("nipples") }}</p>
```

Here, `pants` get passed as `value`, and `nipples` as `extra`.

## Access filters in the config file

Here the filter is accessed, and passed an argument with: `${eleventyConfig.getFilter("url")(url)`

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addShortcode("myCustomImage", function (url, alt) {
    return `<img src="${eleventyConfig.getFilter("url")(url)}" alt="${alt}">`;
  });
};
```

## 11ty data in filters

Some of the 11ty data available in template files, is also available in filters as:

- `this.page`
- `this.eleventy`

See Data Available in templates, [above](#data-available-in-templates).

```js
module.exports = function (eleventyConfig) {
  // Make sure you’re not using an arrow function here: () => {}
  eleventyConfig.addFilter("myFilter", function () {
    // this.page
    // this.eleventy
  });
};
```

## Async filters

These can be used with JavaScript, Nunjucks, and Liquid templates, but not with Handlebars templates.

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addFilter("myFilter", async function (value) {
    // do some Async work
    return value;
  });

  eleventyConfig.addAsyncFilter("myFilter", async function (value) {
    // do some Async work
    return value;
  });
};
```

`addNunjucksAsyncFilter` is also available. **Note, Nunjucks addNunjucksAsyncFilter requires the use of callbacks for async behavior.** See [here](https://www.11ty.dev/docs/languages/nunjucks/#asynchronous-nunjucks-filters).
