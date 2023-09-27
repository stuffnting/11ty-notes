# Templates: markdown files

## Options

The list of options is on [markdown-it GitHub page](https://github.com/markdown-it/markdown-it#init-with-presets-and-options). Some 11ty defaults differ from the defaults of markdown-it. For example, by default 11ty allows HTML in Markdown files.

Note, indented Code Blocks are disabled for both the default Markdown library instance and any set via setLibrary. Although, they can be [reenabled](https://www.11ty.dev/docs/languages/markdown/#indented-code-blocks).

To set a custom instance of the Markdown library, use:

```js
const markdownIt = require("markdown-it");

module.exports = function (eleventyConfig) {
  let options = {
    html: true,
    breaks: true,
    linkify: true,
  };

  eleventyConfig.setLibrary("md", markdownIt(options));
};
```

Or, amend the library instance with:

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.amendLibrary("md", (mdLib) => mdLib.enable("code"));
};
```

## mark-it plugins

11ty supports adding [markdown-it](https://www.npmjs.com/search?q=keywords:markdown-it-plugin) plugins, that can add features to MD processing. Plugins include adding classes, header anchors and wiki text.

See [here](https://www.11ty.dev/docs/languages/markdown/#add-your-own-plugins) for installation instructions.

For an example of using a plugin to add classes and container elements to MD files, see [here](https://davidea.st/articles/11ty-tips-i-wish-i-knew-from-the-start/#4-supercharge-markdown-with-containers-and-classes).

## Markdown in shortcodes

Markdown can be returned in shortcode, by not when it is wrapped in HTML. See [here](https://www.11ty.dev/docs/languages/markdown/#why-cant-i-return-markdown-from-paired-shortcodes-to-use-in-a-markdown-file) for details.
