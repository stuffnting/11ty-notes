# Render plugin

The documentation is [here](https://www.11ty.dev/docs/plugins/render/).

This plugin adds two async shortcodes, `renderTemplate` and `renderFile`, to Nunjucks, Liquid and 11ty.js templates/layouts.

The Render plugin is a built-in plugin that is included automatically. However, to use it, it must be registered in the config file.

```js
const { EleventyRenderPlugin } = require("@11ty/eleventy");

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(EleventyRenderPlugin);
};
```

### `renderTemplate`

The `renderTemplate` shortcode allows regions of a template to be rendered using a different templating engine to the rest of the file.

For example the following will render the shortcode contents as Markdown.

```hbs
{% renderTemplate "md" %} # I am a title * I am a list * I am a list {% endrenderTemplate %}
```

The first argument can be any value that can be used with `templateEngineOverride`. When Markdown is being specified, a pre-processor template engine can be specified as well as Markdown: `"njk,md"`. The one exception here is that `{% renderTemplate "11ty.js" %}` JavaScript string templates are not yet supported—use renderFile below instead.

The second argument can be used as to pass in data. The `eleventy` and `page` variables are available inside the shortcode, but other values need to be passed in.

```hbs
--- myData: myKey: pants --- {% renderTemplate "liquid", myData %}
{{myKey}}
{% endrenderTemplate %}
```

Or,

```hbs
{% renderTemplate "njk,md", {myKey: "pants"} %} # I am a title * I am a list
{{myKey}}
* I am a list {% endrenderTemplate %}
```

### `renderFile`

The `renderFile` shortcode can be used to render a template file from within another template/layout file. **Note, the front matter in the file being rendered will be ignored.**

The template engine used to render the file is chosen using the filename extension.

```njk
{%renderFile "src/_includes/render-me.md"%}
```

Like `renderTemplate`, `eleventy` and `page` variables are available inside the template, other data can be passed in using the second argument.

`renderFile` also has a third argument to override the template engine used to process the file. For example a Nunjucks could be used to process a Markdown file.

```hbs
---
myData: key: value
---

{% renderFile "./_includes/blogpost.md", myData, "njk" %}
```
