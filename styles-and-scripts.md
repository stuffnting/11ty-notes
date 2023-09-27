
# Styles and scripts

## Linking to styles and scripts

CSS and JS files need to be copied through to `_site`. This is done from the 11ty config file: `.eleventy.js`.

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addPassthroughCopy("styles.css");
  eleventyConfig.addPassthroughCopy("index.js");
};
```

This will copy the CSS and JS from the project's root folder, to the root of the `_site` folder. Using `css/styles.css`, copies from `/css/styles.css`, to `_site/css/styles.css`. For more about how to copy from and to different locations, see [here](https://www.11ty.dev/docs/copy/).

When these files are linked to in to HTML files, a path from the root of the `_site` folder must be used.

The following can be added to a template file used to display content from any sub-folder in `_site`, where the CSS and JS files are in the root of the `_site` folder.

```html
<link rel="stylesheet" href="/styles.css" />
<script src="/index.js"></script>
```

If the the styles were in a separate `css` folder, the path would be `/css/styles.css`.

## Inline styles and scripts

These can be added using the Nunjucks' `include` tag. Note, `include` looks in the `_includes` folder, but paths relative to the template file can be used with `includes`, rather than `include`. See [here](https://www.11ty.dev/docs/languages/nunjucks/#supported-features) for more. You can minify inline CSS, see [below](#minifying-css).

```hbs
<style>
  {% include "css/header.css" %}
</style>
```

## Combining stylesheets and scripts

Style and script files can be concatenated into a single file using Nunjucks' `include` tag.

The files are to be added to a `.njk` file, which needs to be in a location from which 11ty will automatically build from: e.g. the root folder of the project, or a 'normal' sub-folder, and not `_includes`. The style combined style file will then be built to the location specified in the YAML `permalink`.

Example, place the following in `/styles/concat-styles.njk``:

```hbs
---
permalink: css/styles.css 
--- 

{% include "css/header.css" %} {% include "css/styles.css" %}
```

The combined stylesheet is built to `_site/css/styles.css`.

Then link from a template file.

```html
<link rel="stylesheet" href="/css/styles.css" />
```

## Minifying CSS

**For details about using Node.js environmental variables to selectively minify the CSS depending on whether `--env` is `production`, or `development`, see [below](#environmental-variables).**

Minifying can be achieved using the clean-css package (this must be installed as a dev dependency), and making a filter in the `.eleventy.js` config file.

```js
const CleanCSS = require("clean-css");

module.exports = function (eleventyConfig) {
  eleventyConfig.addFilter("cssmin", function (code) {
    return new CleanCSS({}).minify(code).styles;
  });
};
```

Then, use the filter in a template file. Either a `.njk` file that concatenates styles:

```hbs
---
permalink: css/styles.css
---

{% set css %}
  {% include "css/header.css" %}
  {% include "css/styles.css" %}
{% endset %}

{{ css | cssmin | safe }}

```

Or, a `.njk` template file that uses inline CSS:

```hbs
{% set css %}
  {% include "sample.css" %}
{% endset %}

<style>
  {{ css | cssmin | safe }}
</style>
```

## Eleventy bundle plugin

This plugin allows for CSS, HTML and JS to be dispersed through a file, and then bundled into a template.

For example, specific CSS for the content in a MD file, or, things to add to the HTML head that are page specific.

See [here](https://github.com/11ty/eleventy-plugin-bundle/) for more.