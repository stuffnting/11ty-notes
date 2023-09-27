# Shortcodes

Shortcodes are available for Markdown, Liquid, Nunjucks, Handlebars and JavaScript template engine. Like filters, shortcodes can be made available to only one template engine, or available to that support shortcodes.

```js
module.exports = function(eleventyConfig) {
  // Shortcodes added in this way are available in:
  // * Liquid
  // * Nunjucks
  // * Handlebars (not async)
  // * JavaScript

  eleventyConfig.addShortcode("user", function(firstName, lastName) { … });

  // Async support for `addShortcode` is new in Eleventy v2.0.0
  eleventyConfig.addShortcode("user", async function(myName) { /* … */ });

  // Async method available
  eleventyConfig.addAsyncShortcode("user", async function(myName) { /* … */ });
};
```

Markdown files are pre-processed as Liquid templates by default (this can be [changed to another templating engine](https://www.11ty.dev/docs/config/#default-template-engine-for-markdown-files)), therefore, they can use shortcodes.

The use of shortcode varies between templating languages, see:

**[JavaScript](https://www.11ty.dev/docs/languages/javascript/#javascript-template-functions):** `*.11ty.js` (with async support). Also `addJavaScriptFunction`

**[Liquid](https://www.11ty.dev/docs/languages/liquid/#shortcodes):** `*.liquid` (with async support). Also `addLiquidShortcode`.

**[Nunjucks](https://www.11ty.dev/docs/languages/nunjucks/#shortcodes):** `*.njk` (with async support). Also `addNunjucksShortcode`.

**[Handlebars](https://www.11ty.dev/docs/languages/handlebars/#shortcodes):** `*.hbs` (no async support). Also `addHandlebarsHelper`.

## Paired shortcodes

In a similar way to WordPress, the content between two shortcode tabs is sent to the callback function.

```hbs
{% user firstName, lastName %} Hello
{{someOtherVariable}}. Hello {% anotherShortcode %}. {% enduser %}
```

The callback function has to be registered as paired in the config file.

```js
module.exports = function(eleventyConfig) {
  // Shortcodes added in this way are available in:
  // * Markdown
  // * Liquid
  // * Nunjucks
  // * Handlebars (not async)
  // * JavaScript

  eleventyConfig.addPairedShortcode("user", function(content, firstName, lastName) { … });

  // Async support for `addPairedShortcode` is new in Eleventy v2.0.0
  eleventyConfig.addPairedShortcode("user", async function(content, myName) { /* … */ });
};
```

## 11ty data in shortcode callbacks

Like filters, shortcodes have access to some 11ty data:

```js
module.exports = function (eleventyConfig) {
  // Make sure you’re not using an arrow function here: () => {}
  eleventyConfig.addShortcode("myShortcode", function () {
    // this.page
    // this.eleventy
  });
};
```

## Events

Code can be run before, or after the build process.

**`eleventy.before`**—Runs before each standalone build, and before each time the build process starts with `--serve` and `--watch`.

**`eleventy.after`**—Runs after each standalone build, and after each time the build process starts with `--serve` and `--watch`.

**Event arguments**—An object passed to the `eleventy.before` and `eleventy.after` containing metadata about the build. See [here](https://www.11ty.dev/docs/events/#event-arguments).

**`eleventy.beforeWatch`**—Only run before an updating build when `--serve`, or `--watch` are used, but it does not run on the initial build, nor a standalone build.

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.on("eleventy.before", async ({ dir, runMode, outputMode }) => {
    // Run me before the build starts
  });
  eleventyConfig.on(
    "eleventy.after",
    async ({ dir, results, runMode, outputMode }) => {
      // Run me after the build ends
    }
  );
  eleventyConfig.on("eleventy.beforeWatch", async (changedFiles) => {
    // Run me before --watch or --serve re-runs
    // changedFiles is an array of files that changed
    // to trigger the watch/serve build
  });
};
```

For details of `dir`, `runMode`, `outputMode`, and `results` see [here](https://www.11ty.dev/docs/events/#event-arguments).
