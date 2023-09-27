# Eleventy - 11ty - v2.0.1

## Resources

Also see:

- the [official documentation](https://www.11ty.dev/docs/getting-started/)
- [11ty Rocks (website)](https://11ty.rocks/). There is a list of other resources, including lots on plugins, at the bottom of the home page.
- [11ty Rocks (YouTube)](https://www.youtube.com/@11tyRocks/videos)
- [Eleventy Walk Through](https://rphunt.github.io/eleventy-walkthrough/intro.html) on GitHub pages
- [11ty.recipes](https://11ty.recipes/)
- [Learn YAML](https://learnxinyminutes.com/docs/yaml/)
- [Nunjucks](https://mozilla.github.io/nunjucks/templating.html)

## Setup

11ty can be run using npx without installing it. The following command starts 11ty, as well as a local dev sever.

```bash
npx @11ty/eleventy --serve
```

To install, use:

```bash
npm install -D @11ty/eleventy
```

The run:

```bash
npx @11ty/eleventy --serve
```

By default the output files are built to `/_site`.

`--serve` starts a web server on port 8080.

### Config files and options

The default config file, `defaultConfig.js`, is on [GitHub](https://github.com/11ty/eleventy/blob/master/src/defaultConfig.js).

For the document action see [here](https://www.11ty.dev/docs/config/).

Many of these options can be applied at the command line, as well as in the config file.

By default 11ty looks for the following files, the first one found is used:

- `.eleventy.js`

- `eleventy.config.js`

- `eleventy.config.cjs`

The options include:

- [`dir.input`](https://www.11ty.dev/docs/config/#input-directory) changes the top level input directory, file, or glob. Note, if an input folder is set, the default it to have `_includes` and `_data` contained within it.

- [`dir.includes`](https://www.11ty.dev/docs/config/#directory-for-includes) changes the folder where 11ty looks for template files. The default is `_includes`. Files in this folder are not processed as template file.

- [`dir.layouts`](<https://www.11ty.dev/docs/config/#directory-for-layouts-(optional)>) it is possible to set a folder for layout, that is separate from the includes folder. Like the includes folder, file in this folder are not processed as template files.

- [`dir.data`](https://www.11ty.dev/docs/config/#directory-for-global-data-files) the folder for global data files.

- [`dir.output`](https://www.11ty.dev/docs/config/#output-directory) the output directory.

- [`markdownTemplateEngine`](https://www.11ty.dev/docs/config/#default-template-engine-for-markdown-files) change the template engine that processes the Markdown files, the default is Liquid. The template engine is used to preprocess the Markdown files, before they are processed as Markdown. Set to `false` to process as Markdown only. The template engine can also be overridden on a per-file basis, using `templateEngineOverride`.

- [`htmlTemplateEngine`](https://www.11ty.dev/docs/config/#default-template-engine-for-html-files) change the template engine that processes the HTML files, the default is Liquid. The template engine is used to preprocess the HTML files. Set to `false` to process as HTML only.

- [`templateFormats`](https://www.11ty.dev/docs/config/#template-formats) an array of template types that should be processed. The default is to process `html`, `liquid`, `ejs`, `md`, `hbs`, `mustache`, `haml`, `pug`,` njk`, and `11ty.js`.

- [`eleventyConfig.setQuietMode(true)`](https://www.11ty.dev/docs/config/#enable-quiet-mode-to-reduce-console-noise) enable quite mode, which limits the amount of information logged during the build process.

- [`pathPrefix`](https://www.11ty.dev/docs/config/#deploy-to-a-subdirectory-with-a-path-prefix) if the site is in a subfolder on the server, this options will add the specified path-prefix to absolute URLs.

- [`htmlOutputSuffix`](https://www.11ty.dev/docs/config/#change-exception-case-suffix-for-html-files) if the input folder is the same as the output folder, a suffix is added to the end of processed HTML files to prevent the template files being overwritten. The default is `-o`.

- [`eleventyConfig.setDataFileBaseName()`](https://www.11ty.dev/docs/config/#change-base-file-name-for-data-files) when using directory specific data files, the default behaviour is for 11ty to look for JS and JSON file names that match the directory name. This can be changed. For example, setting `index` will make 11ty look for `index.json` and `index.11tydata.json` instead of using folder names.

- [`eleventyConfig.setDataFileSuffixes()`](https://www.11ty.dev/docs/config/#change-file-suffix-for-data-files) when using directory and template specific data files, change the default `.11ty.` suffix.

- [`eleventyConfig.addTransform()`](https://www.11ty.dev/docs/config/#transforms) can be used to transform a template's output. For example, format/prettify HTML files.

- [`eleventyConfig.addLinter()`](https://www.11ty.dev/docs/config/#linters) add linters to analyse template output.

- [`eleventyConfig.dataFilterSelectors`](https://www.11ty.dev/docs/config/#data-filter-selectors) include data from the data cascade in the output.

- [`/** @param {import("@11ty/eleventy").UserConfig} eleventyConfig */`](https://www.11ty.dev/docs/config/#type-definitions) adding this at the top of the config file will add extra autocomplete features to some IDEs.

### Command line options

See [here](https://www.11ty.dev/docs/usage/).

There several options can can be used when building eleventy, including:

- `--formats=md,html,ejs` choosing a subset of template types.

- `--serve` start a web server (port 8080) and rebuild when template files are changed are saved. For dev server options see [here](https://www.11ty.dev/docs/dev-server/)

- `--watch` rebuild when template files are changed, but does not start a web server.

- `--incremental` only rebuild the files that have changed, not the whole site, when using `--serve`, and `--watch`. More about incremental builds [here](https://www.11ty.dev/docs/usage/incremental/).

- `--ignore-initial` ignore the initial build when using `--serve`, and `--watch`.

- `--quite` limit the amount of information logged during the build.

- `--dryrun` simulate a build without writing tot he output folder.

- `--config=myeleventyconfig.js` use a different config file, and not `.eleventy.js`.

- `--to=json` output the site as JSON.

- `--input` and `output` use the same folder for input and output—use with care. See [here](https://www.11ty.dev/docs/usage/#using-the-same-input-and-output).

### Delete pervious files on each build

To delete the perviously built files each time the build script is started (but not pon each refresh, when watched files change), add the following script to `package.json`:

```json
"scripts": {
  "build": "rm -rf _site/ && eleventy --serve"
},
```

### Debug mode

Debug mode gives lots of details about the build process. See here for [more](https://www.11ty.dev/docs/debugging/).

### Watch and Serve

For dev server options see [here](https://www.11ty.dev/docs/dev-server/).

`--serve` and `--watch` will automatically watch template files for changes. Data files and JS files, including the config file, are also watched.

Other files and directories can be added to the watch list. Globs can also be used to watch for changes in file types such as `**/*.(png|jpeg)`.

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addWatchTarget("./src/scss/");
};
```

By default 11ty ignores changes to files in `.gitignore`. There is also a default ignore list, that initially, contains `node_modules`; although, the settings can be configured to watch for changes in JS dependencies. Other things can be added to, and removed from the ignore list with:

```js
module.exports = function (eleventyConfig) {
  // Do not rebuild when README.md changes (You can use a glob here too)
  eleventyConfig.watchIgnores.add("README.md");

  // Or delete entries too
  eleventyConfig.watchIgnores.delete("README.md");
};
```

The the live server and build process grind to a halt try the [`--incremental`](https://www.11ty.dev/docs/usage/incremental/) option, or use the [Vite plugin](https://www.11ty.dev/docs/server-vite/).



## Template files: the basic build process

### File type nomenclature

**_Template_**

A content file written in a format such as Markdown, HTML, Liquid, Nunjucks, and more, which Eleventy transforms into a page (or pages) in the built site.

**_Layout_**

A template which wraps around another template, typically to provide the scaffolding markup for content to sit in. These files are not proccessed unless they are called by another files.

**_Partial_**

Part of a layout.


### Default folders

See [here](https://github.com/11ty/eleventy/blob/master/src/defaultConfig.js).

| 11ty dir key | Folder     | Notes                                             |
| ------------ | ---------- | ------------------------------------------------- |
| input        | .          | Start building templates from the project's root. |
| output       | \_site     | Build to here.                                    |
| includes     | \_includes | Layout files. Not automatically built.            |
| data         | \_data     |                                                   |

### Default build

By default the site is built to `/_site`, and 11ty will start to build from `/index.html` in the project's root directory. Other files in the root directors with recognised extensions, such as `html`, `md`, and `nkj` (Nunjucks), will also be built automatically to HTML files, placed in a separate folder within `_site`, and all renamed `index.html`.

```
├── _site
│   ├── README
│   │   └── index.html
│   ├── about
│   │   └── index.html
│   └── index.html
├── about.njk
├──  index.html
└── README.md
```

The full list of recognised template types that will be processed is: `html`, `liquid`, `ejs`, `md`, `hbs`, `mustache`, `haml`, `pug`,` njk`, and `11ty.js`.

Markdown and HTML files are preprocessed by a template engine, before they are treated as MD or HTML. By default the template engine is Liquid. This can be changed globally, in the [config file](#config-files-and-options), or on a per-template basis using `templateEngineOverride` in the front matter.

Files to be included in the build, such as image files and CSS, can be copied to the build folder using [`passThroughCopy`](#passthrough-copy).

11ty with also automatically process the contents of root folders, unless the folder name begins with `_`, or `.`. See the blog example [below](#simple-blog-setup).

For example:

|            |                                     |
| ---------- | ----------------------------------- |
| **Input**  | `sub-dir/template.njk `             |
| **Output** | `_site/sub-dir/template/index.html` |
| **Href**   | `/sub-dir/template/`                |

Or,

|            |                                     |
| ---------- | ----------------------------------- |
| **Input**  | `subdir/template/template.njk`      |
|            | or `subdir/template/index.njk`      |
| **Output** | `_site/sub-dir/template/index.html` |
| **Href**   | `/sub-dir/template/`                |

### Build to custom locations: `permalink`

For changing the output folder, see [here](#config-files-and-options).

The default location where the template files will build to can be changed using the front matter `permalink` key. This special data keys allows variables and shortcodes.

To write to `_site/this-is-a-new-path/subdirectory/testing/index.html`, use:

```yaml
---
permalink: "this-is-a-new-path/subdirectory/testing/"
---
```

Note, `index.html` can be added to the `permalink` path, but is not required.

Template syntax and variables can be use:

```yaml
---
title: This is a New Path
permalink: "sub-dir/{{ title | slugify }}/index.html"
---
```

Writes to `_site/sub-dir/this-is-a-new-path/index.html`.

Dates can also be added:

```yaml
___
permalink: "/{{ page.date }}/{{ page.filePathStem }}
___
```

**If your permalink uses template syntax, make sure that you use quotes.**

If a page template is in a sub-directory, but you want it to appear as if it is in the root, use:

```yaml
___
permalink: "/{{ page.fileSlug }}/"
___
```

#### Build a file to a destination outside the build folder

```yaml
---
permalink: "_includes/index.html"
permalinkBypassOutputDir: true
---
```

This will write to `/_includes/index.html` in the root, and not `/_site/_includes/index.html`.

### Custom build file types

It is possible to build to file types other than HTML. For example, it is possible to build a JSON file from an object:

```hbs
---
permalink: "index.json" 
--- 

{% JSON.stringify(collections.all) %}
```

Or, a text file:

```yaml
---
permalink: "/{{ page.fileSlug }}.txt"
---
```

### Changing how markdown and HTML files are dealt with

Markdown and HTML files are preprocessed using a template engine, by default Liquid. This can be changed globally in the [config file](#config-files-and-options).

Which template engine is used can also be overridden on a per-template basis, using `templateEngineOverride`.

Markdown files are unique in that more than one template engine can be specified, and the files wil be processed in order. Or, specify just `md` have no preprocessing.

```yaml
---
templateEngineOverride: njk,md
---
```

Setting `templateEngineOverride` to `false` on any template file will prevent it from being processed, and the file will be copied to the output folder.

To process the contents of a template file, without it being written to the build folder (by default `_site`), use:

```yaml
---
permalink: false
---
```

### Build from JS files

11ty can also build from JS files, known as 11ty.js templates. The file names should be in form: `built-file-name.11ty.js`. And, the file should be in a location that 11ty builds from automatically, such as the project's root folder.

For example, if the file is `/about.11ty.js`, the built file will be `_site/about/index.html`.

The contents of the JS file are in the form:

```js
module.exports = {
  data: {
    layout: "base-template.njk",
    title: "About",
  },
  render(data) {
    return `<h1>${data.title}</h1>`;
  },
};
```

### Ignore files when building

See [here](https://www.11ty.dev/docs/ignores/) for details.

- Files and folders to be omitted from the build process can be put in a `.eleventyignore` file. `.eleventyignore` files can be placed in the project's root, or the input directory (if it is different from the root).

- Paths listed in your project’s `.gitignore` file are automatically ignored.

- `node_modules` is always ignored.

- The list of files and directories to be ignored are in `eleventyConfig.ignores`. Items can be added and removed from the list in the config file using: `()` and `eleventyConfig.ignores.delete()`.

**_Note, the file watcher has an ignore list which is separate to the build process' watch list._**

### Passthrough copy

The paths for `addPassthroughCopy` are relative to the root, and not the 11ty input folder.

```js
module.exports = function (eleventyConfig) {
  // Output directory: _site

  // Copy `img/` to `_site/img`
  eleventyConfig.addPassthroughCopy("img");

  // Copy `css/fonts/` to `_site/css/fonts`
  // Keeps the same directory structure.
  eleventyConfig.addPassthroughCopy("css/fonts");

  // Copy any .jpg file to `_site`, via Glob pattern
  // Keeps the same directory structure.
  eleventyConfig.addPassthroughCopy("**/*.jpg");
};
```

If the source folder for the passthrough files is inside the 11ty input folder, the input folder is stripped from the output path.

For example, if the input folder is `/src`, and the passthrough files are in `/src/img`, the passthrough files are copied to `_site/img`, and not `_Site/src/img`.

`addPassthroughCopy` can:

- use globs to specify to find files to copy;

- change the output directory;

- use globs to copy all files of a certain type to a specified directory;

- emulate passthrough when using `--serve`, to speed up build times.

**See [here](https://www.11ty.dev/docs/copy/) for more.**

#### Passthrough by file extension

`setTemplateFormats` specifies which file extensions to process. The default list is: `html`,`liquid`,`ejs`,`md`,`hbs`,`mustache`,`haml`,`pug`,`njk`, and `11ty.js`. If file types are added to this list that are not template types, the files are copied, rather than processed.

By using `setTemplateFormats`, files with specified extensions can be copied automatically, without using `addPassthroughCopy`. Note, the array replaces the default values, therefore, all of the template extensions that you are using need to be added.

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.setTemplateFormats([
    "md",
    "njk",
    "html",
    "css", // css is not by default a recognized template extension in Eleventy
  ]);
};
```


## Simple build in styles and scripts

### Linking to styles and scripts

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

### Inline styles and scripts

These can be added using the Nunjucks' `include` tag. Note, `include` looks in the `_includes` folder, but paths relative to the template file can be used with `includes`, rather than `include`. See [here](https://www.11ty.dev/docs/languages/nunjucks/#supported-features) for more. You can minify inline CSS, see [below](#minifying-css).

```hbs
<style>
  {% include "css/header.css" %}
</style>
```

### Combining stylesheets and scripts

Style and script files can be concatenated into a single file using Nunjucks' `include` tag.

The files are to be added to a `.njk` file, which needs to be in a location from which 11ty will automatically build from: e.g. the root folder of the project, or a 'normal' sub-folder, and not `_includes`. The style combined style file will then be built to the location specified in the YAML `permalink`.

Example, place the following in `/styles/concat-styles.njk``:

```hbs
--- permalink: css/styles.css --- {% include "css/header.css" %} {% include
"css/styles.css" %}
```

The combined stylesheet is built to `_site/css/styles.css`.

Then link from a template file.

```html
<link rel="stylesheet" href="/css/styles.css" />
```

### Minifying CSS

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

### Eleventy plugin bundle

This plugin allows for CSS, HTML and JS to be dispersed through a file, and then bundled into a template.

For example, specific CSS for the content in a MD file, or, things to add to the HTML head that are page specific.

See [here](https://github.com/11ty/eleventy-plugin-bundle/) for more.

## Front matter

[YAML](https://learnxinyminutes.com/docs/yaml/) is the default for front matter, however, other formats can be used; even custom formats are possible. See [here](https://www.11ty.dev/docs/data-frontmatter/#alternative-front-matter-formats) for more.

The items in the front matter are made available to the layout file.

There are some default front matter keys (in `defaultConfig.js` on [GitHub](https://github.com/11ty/eleventy/blob/master/src/defaultConfig.js)):

```js
keys: {
  package: "pkg",
  layout: "layout",
  permalink: "permalink",
  permalinkRoot: "permalinkBypassOutputDir",
  engineOverride: "templateEngineOverride",
  computed: "eleventyComputed",
},
```

Also, `date` can override the content file's Created Date when ordering collections (see [below](#sorting-collections)).

### Basic use

This is a sample of basic use, however, there are lots ways to add data (see [here](https://learnxinyminutes.com/docs/yaml/)).

```yaml
---
layout: mainlayout.njk
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

### Front matter formats

Eleventy uses [the gray-matter package](https://github.com/jonschlinkert/gray-matter) for front matter processing. gray-matter (and thus, Eleventy) includes support out of the box for YAML, JSON, and even JavaScript object literals in front matter.

**_YAML_**

```yaml
---
title: My page title
---
```

**_JSON_**

```json
---json
{
  "title": "My page title"
}
---
```

**_JS object literal_**

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

### Custom front matter formats

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

### Template strings in YAML front matter

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

### Bulk set front matter

Front matter can be set for a whole folder using a JSON file. See the blog example [below](#simple-blog-setup).

## Layouts

### Template file location

Files in the `_includes` folder are not processed as full template files, that will become separate files in `_site`, they are used by other templates.

11ty template files can be stored in a different folder, which is specified in the config file (see [here](<https://www.11ty.dev/docs/config/#directory-for-layouts-(optional)>)).

### File type

The template file to use for different content is specified in the YAML front matter. The path to the template file is relative to `_includes`.

If variables are included in HTML file, 11ty seems to default to Liquid to deal with them.

#### The content

Content can be in lost of mixed and matched forms.

**Markdown**

```yaml
---
layout: mylayout.njk
title: My Rad Markdown Blog Post
---
# {{ title }}
```

**Nunjucks/Liquid**

```hbs
--- layout: mylayout.njk title: My Rad Nunjucks Blog Post ---
<h1>{{title}}</h1>
```

**11ty.js**

```js
module.exports = {
  data: {
    layout: "mylayout.njk",
    title: "My Rad JavaScript Blog Post",
  },
  render(data) {
    return `<h1>${data.title}</h1>`;
  },
};
```

#### The layout

The `_includes/mylayout.njk` file.

```
---
title: My Rad Blog
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
  </head>
  <body>
    {{ content | safe }}
  </body>
</html>
```

`safe` is a [Nunjucks filter](https://mozilla.github.io/nunjucks/templating.html#safe), which prevents escaping of the content. If `safe` is not used, the content will be escaped.

Note, layout files can contain their own front matter, which is merged with the content's data; the content's data takes precedence.

### Default layout

In 11ty there is no way to set a default layout file. Therefore, if `layout` is not defined for a template, the page will consist only of the HTML within the template file.

To get around this, make a file called `_data/layout.js` and add:

```js
module.exports = 'layout.njk';
```

Now, any template without a `layout` specified will use this file.
 
### Layout aliasing

If multiple files all have the same layout specified, and you want to easily change all of them to use a different layout, the following can be added to the `.eleventy.js` config file.

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addLayoutAlias("post", "layouts/post.njk");
};
```

### Extensionless layout files

The layout file's extension can be omitted, although this can be problematic. and this can be disabled. See [here](https://www.11ty.dev/docs/layouts/#omitting-the-layouts-file-extension) for more.

### Chaining layout files

Layout files can be chained, so that layout files, themselves, use other layout files.

**_Template file_**

```yaml
---
layout: mainlayout.njk
title: My Rad Blog
---
# My Rad Markdown Blog P
```

**_mainlayout.njk_**

```hbs
---
layout: mylayout.njk
myOtherData: hello
---
<main>
  {{ content | safe }}
</main>
```

**_mylayout.njk_**

```hbs
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{title}}</title>
  </head>
  <body>
    {{ content | safe }}
  </body>
</html>
```

### Logging data from template and layout files

**Built-in log filter**

**Use Nunjucks `dump` filer**

This dumps the data into the template.

```
{{ items | dump }}
```

**Make your own `log` filer and dump to the console**

In the `.eleventy.js` config file:

```js
eleventyConfig.addFilter("log", (value) => {
  console.log(value);
});
```

And, then in the template/layout file:

```hbs
{{ article | log }}
<li class="article"> ... </li>
```

## Collections

Collections are pieces of content that are grouped together using `tags` in their front matter. A piece of content can be in multiple collections.

### Adding tags

**_A single tag_**

```yaml
---
tags: post
title: Word to the wise: a blog post
---
```

**_Multiple tags as an array_**

```yaml
---
tags: ["cat", "dog"]
---
```

**_Multiple tags on multiple lines_**

```yaml
---
tags:
  - cat
  - dog
---
```

### Simple blog setup

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

### The `all` collection

All content is placed within the `collections.all` collection, whether tgs were assigned to it or not.

### `eleventyExcludeFromCollections`

This can be used to exclude content from all collections, even when `tags` are set.

```yaml
---
eleventyExcludeFromCollections: true
tags: post
---
This will not be available in `collections.all` or `collections.post`.
```

### Collection data structure

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

**_Note_**

- `page`: everything in Eleventy’s supplied `page` variable for this template (including `inputPath`, `url`, `date`, and `others`). **Use these values n preference to the top-level values.** (Added in v2.0.0.)

- `data`: all `data` for this piece of content (includes any `data` inherited from layouts, template specific, layout specific, and global data). Note, the `data` keys are available in collection templates as variables, e.g. {{ `date` }}, {{ `title` }} etc., but when creating archive pages via `pagination`, they are accessible through `item.data.title` etc. (where `item` is the iteration variable)

- `content`: the rendered `content` of this template. This does not include layout wrappers. (Added in v2.0.0)

Backwards compatibility:

- Top level properties for ` inputPath``,  `fileSlug`, `outputPath`, `url`, `date`are still available, though use of`page.\*` (Added in v2.0.0) for these is encouraged moving forward.

- `content` (Added in v2.0.0) is aliased to the previous property `templateContent`.

### Sorting collections

**_Default_**

The default sorting is by the files Created Date. Files have the same Created Date, their full paths are used to sort them. See here for [more](https://www.11ty.dev/docs/collections/#sorting).

**_Sort descending_**

This can be done with the Nunjucks `reverse` filter ([other templating options](https://www.11ty.dev/docs/collections/#sort-descending)).

```hbs
<ul>
  {%- for post in collections.post | reverse -%}
  <li>{{post.data.title}}</li>
  {%- endfor -%}
</ul>
```

### Override the Created Date

This is achieved using `date` in the content's front matter.

```yaml
---
date: 2016-01-01
---
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

#### Example

This example defines a collection that includes all of the MD files from the `/posts`, and `/more-templates` folders.

**_`.eleventy.js`_**

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

**_Layout file_**

```hbs
{% for post in collections.moreStuff %}

<h2><a href="{{ post.url }}">{{ post.data.title }}</a></h2>

<p>{{ post.content | safe }}</p>

{% endfor %}
```

## Create pages from data

See [here](https://rphunt.github.io/eleventy-walkthrough/pagination.html).

Pages can be created from data stored within JSON files, which are kept in the `_data` folder. Pages can also be made from data with a template's front matter.

The first few examples deal with making one page per data item. For having more than one data item per page, see [Multiple data items per page](#multiple-data-items-per-page).

### Automatic permalinks—JSON

**_`_data/possum-pages.json`:_**

```json
[
  {
    "name": "Fluffy",
    "age": 2
  },
  {
    "name": "Snugglepants",
    "age": 5
  },
  {
    "name": "Lord Featherbottom",
    "age": 4
  },
  {
    "name": "Pennywise",
    "age": 9
  }
]
```

**_/possum-pages.njk_**

The page template below the front matter is called for each new page, in this case that means once for each data item.

```hbs
--- pagination: data: possums size: 1 alias: possum ---

{{possum.name}}
is
{{possum.age}}
years old
```

- `data`—the file in `_data` t ouse as the source.

- `size`—how many items to put on each page created. To create a page per item, use 1. For details bout having more than one item on a page see [below](#multiple-data-items-per-page).

- `alias`—a variable name used to refer to the data item currently being turned into a page. This is an alias for `pagination.items[0]` etc.

Note, the above code can be written with a conventional Nunjucks for loop. in Which case, the `alias` is not needed. Despite there being only one data item to display per page, a loop is used, which is similar to the way WorPress displays a single post/page.

```hbs
--- pagination: data: possums size: 1 alias: possum --- {% for possum in
pagination.items %}
{{possum.name}}
is
{{possum.age}}
years old. {% endfor %}
```

**_Pages created_**

The above code produces a folder `_site/possum-pages`, where the file `_site/possum-pages/index.html` contains the first data item ("Fluffy").

`_site/possum-pages` also contains three sub-folders, numbered 1 to 3, each of which contains an `index.html` file. These files contain the 2nd, 3rd and 4th data items, respectively.

### Automatic permalinks—YAML

The data can be supplied from the front matter.

**_`my-people.njk`_**

```hbs
--- pagination: data: testdata size: 1 testdata: - item1: name: Bob age: 22 -
item2: name: Mike age: 33 - item3: name: Dave age: 44 - item4: name: Bill age:
55 --- {% for person in pagination.items %}
<li>{{person.name}} is {{person.age}} old.</li>
{% endfor %}
```

As with the previous section, `_site/my-people/index.html` contains the first person, and the there are 3 sub-folders, numbered 1 to 3, for the other people.

### Custom permalinks

There are more examples [here](https://www.11ty.dev/docs/pagination/#remapping-with-permalinks).

Custom permalinks can be used for the pages created from a data source.

```hbs
---
pagination:
  data: possums
  size: 1
  alias: possum
permalink: "possums/{{ possum.name | slugify }}/"
---

<main>
  {{ possum.name }} is {{ possum.age }} years old
</main>
```

This time `_site/possum-pages` does not contain an `index.html` file, and there are now four sub-folders—one for each item—named using the `name` fields in the JSON file.

### Multiple data items per page: `size` > 1

**_\_data/multi-data-items.json_**

```json
[
  {
    "id": 1,
    "name": "France",
    "continent": "Europe",
    "special-move": "Cheese"
  },
  {
    "id": 2,
    "name": "Germany",
    "continent": "Europe",
    "special-move": "Sausage"
  },
  {
    "id": 3,
    "name": "USA",
    "continent": "North America",
    "special-move": "Ultra-processed foods"
  },
  {
    "id": 4,
    "name": "Mexico",
    "continent": "North America",
    "special-move": "Taco"
  },
  {
    "id": 5,
    "name": "Argentina",
    "continent": "South America",
    "special-move": "Corned Beef"
  },
  {
    "id": 6,
    "name": "India",
    "continent": "Asia",
    "special-move": "Curry"
  },
  {
    "id": 7,
    "name": "China",
    "continent": "Asia",
    "special-move": "Noodles"
  }
]
```

**_/country-food.njk_**

```hbs
--- pagination: data: multi-data-items size: 2 ---

<ol>
  {% for country in pagination.items %}
  <li>
    <ul>
      <li>Country: {{country.name}} </li>
      <li>Continent: {{country.continent}}</li>
      <li>Special Move: {{country.special-move}}</li>
    </ul>
  </li>
  {% endfor %}
</ol>
```

This code creates the `_site/country-food/` folder, inside which is `index.html`, that contains the first 2 countries. Three sub-folders are created, numbered 1 to 3. The `index.html` files in sub-folders `1` and `2` contain two items each. And, the `index.html` file in the last sub-folder (3), contains the last data item, which is `China`.

### `alias` with multiple data items

When more than one data item is being included per page, the `alias` will refer to an array.

```hbs
---
pagination:
  data: testdata
  size: 2
  alias: rainbow
testdata:
  - Rod
  - Jane
  - Freddy
  - Zippy
permalink: "different/{{ [rainbow[0], rainbow[1]] | join('-') | slugify }}/index.html"
---

<ol>
  <li>You can use the alias in your content too {{ rainbow[0] }}.</li>
  <li>You can use the alias in your content too {{ rainbow[1] }}.</li>
</ol>
```

Here, the Nunjucks `join` filer is used to concatenate the two items in the `alias` array to forma a single directory name.

### `alias` where each item is an object

```hbs
---
pagination:
  data: testdata
  size: 2
  alias: bloke
testdata:
  - item1:
    name: Bob
    age: 22
  - item2:
    name: Mike
    age: 33
  - item3:
    name: Dave
    age: 44
  - item4:
    name: Bill
    age: 55
---

<ol>
  <li>{{ bloke[0].name }} is {{ bloke[0].age }} old.</li>
  <li>{{ bloke[1].name }} is {{ bloke[1].age }} old.</li>
</ol>
```

In this example, for each page created `block` will contain an array containing 2 objects.

```js
// 1st page
[
  { item1: null, name: "Bob", age: 22 },
  { item2: null, name: "Mike", age: 33 },
][
  // 2nd page
  ({ item3: null, name: "Dave", age: 44 },
  { item4: null, name: "Bill", age: 55 })
];
```

### Paginating an object—keys

This time the global data file contains an object of objects, rather than an array of objects.

By default, 11ty will make a pagination array using `Object.keys()`. This array is then used to loop through the data items, and can also be used to reference the nested objects in the original data.

**_\_data/multi-data-items.json_**

```json
{
  "fr": {
    "id": 1,
    "name": "France",
    "continent": "Europe",
    "special-move": "Cheese"
  },
  "de": {
    "id": 2,
    "name": "Germany",
    "continent": "Europe",
    "special-move": "Sausage"
  },
  "us": {
    "id": 3,
    "name": "USA",
    "continent": "North America",
    "special-move": "Ultra-processed foods"
  },
  "mx": {
    "id": 4,
    "name": "Mexico",
    "continent": "North America",
    "special-move": "Taco"
  },
  "ar": {
    "id": 5,
    "name": "Argentina",
    "continent": "South America",
    "special-move": "Corned Beef"
  },
  "in": {
    "id": 6,
    "name": "India",
    "continent": "Asia",
    "special-move": "Curry"
  },
  "cn": {
    "id": 7,
    "name": "China",
    "continent": "Asia",
    "special-move": "Noodles"
  }
}
```

**_/country-food.njk_**

```hbs
---
pagination:
  data: multiDataItems
  size: 2
---

<ol>
{%- for country in pagination.items %}
  <li>{{ country }}={{multiDataItems[country].name }}</li>
{% endfor -%}
</ol>
```

Here, `multiDataItems[country].name` gets the value for name from the original global data object.

### Paginating an object—values

By setting `resolve` to `values`, 11ty will use `Object.values()` to make the pagination array.

This example uses the same global data object as the last example.

**_/country-food.njk_**

```hbs
---
pagination:
  data: multiDataItems
  size: 2
  resolve: values
---

<ol>
{%- for country in pagination.items %}
  <li>{{ country.name }} = {{ country['special-move'] }}</li>
{% endfor -%}
</ol>
```

## Modifying the data before pagination

### Reverse the data

```yaml
---
pagination:
  data: testdata
  size: 2
  reverse: true
testdata:
  - item1
  - item2
  - item3
  - item4
---
```

Results in:

```js
[
  ["item4", "item3"],
  ["item2", "item1"],
];
```

### Filter values

**_Resolving an object by key:_**

```yaml
---
pagination:
  data: testdata
  size: 1
  filter:
    - item3
testdata:
  item1: itemvalue1
  item2: itemvalue2
  item3: itemvalue3
---
```

Results in:

```js
[["item1"], ["item2"]];
```

**_Resolving an object by value:_**

```yaml
---
pagination:
  data: testdata
  size: 1
  resolve: values
  filter:
    - itemvalue3
testdata:
  item1: itemvalue1
  item2: itemvalue2
  item3: itemvalue3
---
```

Results in:

```js
[["itemvalue1"], ["itemvalue2"]];
```

### Use a callback to filter the data

Using a callback function it is possible to filter the data in anyway desired.

See [here](https://www.11ty.dev/docs/pagination/#the-before-callback).

### The order of modification operations

If you use more than one of these data set modification features, here’s the order in which they operate:

- The `before` callback

- `reverse: true`

- `filter entries`

## The `pagination` object

### `pagination` object example

The `pagination` object for the following example is explored below.

```hbs
--- pagination: data: testdata size: 2 testdata: - item1: name: Bob age: 22 -
item2: name: Mike age: 33 - item3: name: Dave age: 44 - item4: name: Bill age:
55 ---

<ol>
  {% for person in pagination.items %}
  <li>{{person.name}} is {{person.age}} old.</li>
  {% endfor %}
</ol>
```

### What's in the `pagination` object

The `pagination` object for each page created looks like this:

```js
{
  // Use this stuff for pagination
  items: [], // Array of current page’s chunk of data
  pageNumber: 0, // current page number, 0 indexed

  // Cool URLs
  hrefs: [], // Array of all page hrefs (in order)
  href: {
    next: "…", // put inside <a href="">Next Page</a>
    previous: "…", // put inside <a href="">Previous Page</a>
    first: "…",
    last: "…",
  },

  pages: [], // Array of all chunks of paginated data (in order)
  page: {
    next: {}, // Next page’s chunk of data
    previous: {}, // Previous page’s chunk of data
    first: {},
    last: {},
  },

  // Other stuff
  data: "…", // the original string key to the dataset
  size: 1, // page chunk sizes

  // Cool URLs
  // Use pagination.href.next, pagination.href.previous, et al instead.
  nextPageHref: "…", // put inside <a href="">Next Page</a>
  previousPageHref: "…", // put inside <a href="">Previous Page</a>
  firstPageHref: "…",
  lastPageHref: "…",

  // Uncool URLs
  // These include index.html file names, use `hrefs` instead
  links: [], // Array of all page links (in order)

  // Deprecated things:
  // nextPageLink
  // previousPageLink
  // firstPageLink
  // lastPageLink
  // pageLinks (alias to `links`)
}
```

Considering the example above:

- `items`—an array of data items to be used ono the current page.

```js
items: [
  { item1: null, name: 'Bob', age: 22 },
  { item2: null, name: 'Mike', age: 33 }
],
```

- `hrefs`—An array containing the relative paths of all pages created.

```js
hrefs: [ '/paged-data/', '/paged-data/1/' ],
```

- `href`—An array containing the relative paths for the `previous`, `next`, `first`, and `last` pages.

```js
{
  previous: null,
  next: '/paged-data/1/',
  first: '/paged-data/',
  last: '/paged-data/1/'
}
```

- `pages`—An array of the data for each page.

```js
[
  [
    { item1: null, name: "Bob", age: 22 },
    { item2: null, name: "Mike", age: 33 },
  ],
  [
    { item3: null, name: "Dave", age: 44 },
    { item4: null, name: "Bill", age: 55 },
  ],
];
```

- `page`—An array of the the data for the `next`, `previous`, `first`, and `last` pages.

```js
{
  previous: null,
  next: [
    { item3: null, name: 'Dave', age: 44 },
    { item4: null, name: 'Bill', age: 55 }
  ],
  first: [
    { item1: null, name: 'Bob', age: 22 },
    { item2: null, name: 'Mike', age: 33 }
  ],
  last: [
    { item3: null, name: 'Dave', age: 44 },
    { item4: null, name: 'Bill', age: 55 }
  ]
}
```

## Creating pages from data and adding them to a collection

By default, only the first page created via pagination will be added to a collection specified in the front matter.

In this example only the page containing Rod and Jane will be added.

```hbs
---
pagination: 
  data: testdata 
  size: 2 
  testdata: 
    - Rod 
    - Jane 
    - Freddy 
    - Zippy 
    - George 
    - Bungle 
    - Geoffrey 
---

<ol>
  {% for character in pagination.items %}
  <li>Also starting {{character}}</li>
  {% endfor %}
</ol>
```

By setting `addAllPagesToCollections: true`, all of the pages created will be added to the collection.

```hbs
---
  tags: 
    - rainbowCollective 
pagination: 
  data: testdata 
  size: 2 
  addAllPagesToCollections: true
  testdata: 
    - Rod 
    - Jane 
    - Freddy 
    - Zippy 
    - George 
    - Bungle 
    - Geoffrey 
---

<ol>
  {% for chatacter in pagination.items %}
  <li>Also starting {{chatacter}}</li>
  {% endfor %}
</ol>
```

The collection contain the pagination object for each page created. The contents of the collection will be the output of the pagination template.

The following template file would output each of the ordered lists created by the pagination.

```hbs
---
layout: layout.html
---

{% for item in collections.rainbowCollective %}
  {{ item.content | safe }}
{% endfor %}
```

`item.data` will contain the rest of the pagination object data.

## Paginating existing collections: making archive pages

To paginate a collection that already exists, rather than adding pages created by pagination to a collection, use the collection as the data source. Doing this, in effect, wraps the collection in the pagination object.

For example, make an archive page of blog posts, that displays 6 on each page.

```hbs
---
title: Latest posts
layout: layout.njk
pagination:
  data: collections.post
  size: 3
---

{% for post in pagination.items %}
  <article>
  <h2>
    {{ post.data.title | default("Emergency default title") | safe }}
  </h2>
  <section>
    {{ post.data.the_excerpt | default(post.content) | safe }}
  </section>
</article>
{% endfor %}
```

**Note, that the title is in `post.data.title`, and not is not accessible via `post.title`, but the `content` is accessible via `post.content`. Also, you might have to use the Nunjucks `safe` filter.**

Example of a blog archive page with next, previous and page navigation:

```hbs
---
title: Latest posts
layout: layout.njk
pagination:
  data: collections.post
  size: 2
---

{% for post in pagination.items %}
<article>
  <h2>
    {{ post.data.title | default("Emergency default title") | safe }}
  </h2>
  <section>
    {{ post.data.the_excerpt | default( post.content ) | safe }}
  </section>
</article>
{% endfor %}

<section>
<nav aria-labelledby="my-pagination">
  <hr />
  <ul>
    <li>{% if page.url != pagination.href.first %}<a href="{{ pagination.href.first }}">First</a>{% else %}First{% endif %}</li>
    <li>{% if pagination.href.previous %}<a href="{{ pagination.href.previous }}">Previous</a>{% else %}Previous{% endif %}</li>
{%- for pageEntry in pagination.pages %}
    <li><a href="{{ pagination.hrefs[ loop.index0 ] }}"{% if page.url == pagination.hrefs[ loop.index0 ] %} aria-current="page"{% endif %}>Page {{ loop.index }}</a></li>
{%- endfor %}
    <li>{% if pagination.href.next %}<a href="{{ pagination.href.next }}">Next</a>{% else %}Next{% endif %}</li>
    <li>{% if page.url != pagination.href.last %}<a href="{{ pagination.href.last }}">Last</a>{% else %}Last{% endif %}</li>
  </ul>
</nav>
</section>
```

## A full list of `pagination` options

From [here](https://www.11ty.dev/docs/pagination/#full-pagination-option-list).

- `data` (String) Lodash.get path to point to the target data set.

- `size` (Number, required)

- `alias` (String) refer to the data set by a different name within the template. When `size` > 1, bracket notation needs to be used. See [here](https://www.11ty.dev/docs/pagination/#aliasing-to-a-different-variable).

- `generatePageOnEmptyData` (Boolean) if target data set is empty, render first page with empty chunk [].

- `resolve: values` ("values") if the data is an object, pagination iterates over the values instead of the keys.

- `filter` (Array) values to remove from the paginated data.

- `reverse: true` (Boolean) if `true`, reverse the oder of the data.

- `addAllPagesToCollections: true` (Boolean) by default only the first page paginated from data will be added to the collection specified in `tags`.

- `before` (Function) callback to modify, filter, or otherwise change the pagination data before pagination occurs.

**Note: when modifying data, the order of operation is:**

- The `before` callback

- `reverse: true`

- `filter` entries

## Pagination navigation

### An index of paginated pages when sourced from an array

In this example there are 7 items being divided at 2 per page. Therefore, 4 pages are made: the first 3 having two items each, and the fourth having only one item.

This code creates a list of the created pages, with link to each page. The current page's link tag is marked with an `aria-current` attribute.

```hbs
---
layout: layout.html
title: The Rainbow Collective
pagination:
  data: testdata
  size: 2
testdata:
  - Rod
  - Jane
  - Freddy
  - Zippy
  - George
  - Bungle
  - Geoffrey
---

<nav aria-labelledby="my-pagination">
  <h2 id="my-pagination">This is my Pagination</h2>
  <ol>
  {% for pageEntry in pagination.pages %}
    <li>
      <a href="{{ pagination.hrefs[ loop.index0 ] }}"
        {% if page.url == pagination.hrefs[ loop.index0 ] %}
          aria-current="page"
        {% endif %}
      >
        Page {{ loop.index }}: {{ pageEntry[0] }}  {% if pageEntry[1]  %} and {{ pageEntry[1] }}{% endif %}
      </a>
    </li>
  {% endfor %}
  </ol>
</nav>
```

The result is:

```html
<nav aria-labelledby="my-pagination">
  <h2 id="my-pagination">This is my Pagination</h2>
  <ol>
    <li>
      <a href="/country-food/" aria-current="page"> Page 1: Rod and Jane </a>
    </li>
    <li><a href="/country-food/1/"> Page 2: Freddy and Zippy </a></li>
    <li><a href="/country-food/2/"> Page 3: George and Bungle </a></li>
    <li><a href="/country-food/3/"> Page 4: Geoffrey and </a></li>
  </ol>
</nav>
```

In the above code:

- The `id` of the `h2` element needs to be unique to the final HTML page it is on.

- A Nunjucks `for` loop is used to loop through the `pagination.pages`.

- `pagination.pages` is an array of `pagination.items` for all the pages create. Since `size` is `2`, it looks like:

```js
[["Rod", "Jane"], ["Freddy", "Zippy"], ["George", "Bungle"], ["Geoffrey"]];
```

- Within a Nunjucks `for` loop, there is access to the `loop` variable. For the first iteration, creating the link to the first page, `loop` contains ([details here](https://mozilla.github.io/nunjucks/templating.html#for)):

```js
{
  index: 1,
  index0: 0,
  revindex: 4,
  revindex0: 3,
  first: true,
  last: false,
  length: 4
}
```

- `loop.index` is the current iteration of the loop (1 indexed), and `loop.index0` is the current iteration of the loop (0 indexed). Hence, the former is used for the link text, and the latter is used in the code to reference an item in the array.

- The 11ty variable `page` is available in template files, what it contains depends on what the template is processing. In this case it contains the built page's URL.

```js
// For the first page
{
  url: '/rainbow-collective/',
  outputPath: '_site/rainbow-collective/index.html'
}

// For the others (the number will change)
{
  url: '/rainbow-collective/1/',
  outputPath: '_site/rainbow-collective/1/index.html'
}
```

- Because `size` is `2`, each `pageEntry` is an array of two items, apart from the last one, which contains only one. The original data items, are therefore, accessed with `pageEntry[0]` and `pageEntry[1]`.

- A Nunjucks `if` tag is used to check that the second item exists, before adding it to the link. The last page will only have item.

### An index of paginated pages when sourced from an object

This is very similar to the array example, except that the object is first turned into an array of the keys, so that it can be iterated. The items from the key array are use to get the corresponding values out of the original object. This is done with, `testdata[ pageKey[0] ]`, where `testdata` is the original array, and `pageKey[0]` is the current item in the kays array.

```hbs
---
layout: layout.html
title: A list of items
pagination:
  data: testdata
  size: 2
testdata:
  France: Cheese
  Germany: Sausage
  Mexico: Taco
  Argentina: Corned Beef
  USA: Burger
  India: Curry
  China: Noodles
---

<ul>
  {% for pageKey in pagination.pages %}
    <li>
      <a href="{{ pagination.hrefs[ loop.index0 ] }}"
        {% if page.url == pagination.hrefs[ loop.index0 ] %}
          aria-current="page"
        {% endif %}
      >
        Page {{ loop.index }}: {{ testdata[ pageKey[0] ] }}
          {% if pageKey[1] %}
            and {{ testdata[ pageKey[1] ] }}
          {% endif %}
      </a>
    </li>
  {% endfor %}
</ul>
```

### Adding Previous, next, first and last links

Using the previous example.

```hbs
<nav aria-labelledby="my-pagination">
  <h2 id="my-pagination">This is my Pagination</h2>
  <ol>
    <li>{% if page.url != pagination.href.first %}<a href="{{ pagination.href.first }}">First</a>{% else %}First{% endif %}</li>
    <li>{% if pagination.href.previous %}<a href="{{ pagination.href.previous }}">Previous</a>{% else %}Previous{% endif %}</li>
{%- for pageEntry in pagination.pages %}
    <li><a href="{{ pagination.hrefs[ loop.index0 ] }}"{% if page.url == pagination.hrefs[ loop.index0 ] %} aria-current="page"{% endif %}>Page {{ loop.index }}</a></li>
{%- endfor %}
    <li>{% if pagination.href.next %}<a href="{{ pagination.href.next }}">Next</a>{% else %}Next{% endif %}</li>
    <li>{% if page.url != pagination.href.last %}<a href="{{ pagination.href.last }}">Last</a>{% else %}Last{% endif %}</li>
  </ol>
</nav>
```

## More templating

### Special front matter data keys

There are a few special data keys you can assign in your data to control how templates behave. These can live anywhere in the Data Cascade, such as front matter, and include:

- `permalink`: Change the output target of the current template. Normally, you cannot use template syntax to reference other variables in your data, but permalink is an exception. Setting `permalink` to `false` allows the page to be processed, but prevents the page being built.
- `layout`: Wrap current template with a layout template found in the `_includes` folder.
- `pagination`: Enable to iterate over data. Output multiple HTML files from a single template.
- `tags`: A single string or array that identifies that a piece of content is part of a collection. Collections can be reused in any other template.
- `date`: Override the default date (file creation) to customize how the file is sorted in a collection.
- `templateEngineOverride`: Override the template engine on a per-file basis.
- `eleventyExcludeFromCollections`: Set to `true` to exclude this content from any and all Collections.
- `eleventyComputed`: Programmatically set data values based on other values in your data cascade. When used in YAML front matter, this allows for all data values, not just `permalink`, to be set using template strings.
- `eleventyNavigation`: Used by the Navigation plugin.
- `eleventyImport`: An array to declare any collections used in the page. This helps with incremental builds, and template build order. See [here](https://github.com/11ty/eleventy/issues/975).

These are all available in template/layout files using their keys, e.g. `{{ date }}`. They also appear in the `data` property of `collections` objects.

**Note, only `permalink` and `eleventyComputed` can contain variable and shortcodes**

If you need to use variables or shortcodes in other front matter values, use `eleventyComputed` to set them.

### Generating an excerpt using `setFrontMatterParsingOptions`

Excerpts can be generated from the template content, and added to the `page` object, which is available in the body of the template.

**_.eleventy.js_**

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    excerpt: true,
    // Optional, default is "---"
    excerpt_separator: "<!-- excerpt -->",
  });
};
```

**_post-1.md_**

```js
---
title: My page title
---
This is the start of my content and this will be shown as the excerpt.
<!-- excerpt -->
This is a continuation of my content…
```

`page.excerpt` now holds This is the start of my content and this will be shown as the excerpt.

For example:

```js
{
  excerpt: '\n' +
    'Magna aliquet suspendisse accumsan aliquet dis purus.\n' +
    '\n',
  date: 2023-07-08T12:24:17.827Z,
  inputPath: './posts/post-2.md',
  fileSlug: 'post-2',
  filePathStem: '/posts/post-2',
  outputFileExtension: 'html',
  templateSyntax: 'liquid,md',
  url: '/posts/post-2/',
  outputPath: '_site/posts/post-2/index.html'
}
```

The except does not need to be stored in `page.excerpt`, and it can be assigned to a custom variable:

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    excerpt: true,
    // Eleventy custom option
    // The variable where the excerpt will be stored.
    excerpt_alias: "my_custom_excerpt",
  });
};
```

Now the excerpt can be accessed in the templates as `my_custom_excerpt`.

### Front matter `date`

The default date used by 11ty is the template file's created date, however, this can be overridden.

```js
date: 2023 - 07 - 12;
```

There is a list of possible values [here](https://www.11ty.dev/docs/dates/#setting-a-content-date-in-front-matter)

If a `date` key is omitted from the file, 11ty looks for a `YYYY-MM-DD` format anywhere in the file path, even folders. If there are multiple dates found, the first is used. As a last resort, the file creation date is used.

Example of using `date` in `permalink`:

```yaml
---
date: "2016-01-01T06:00-06:00"
permalink: "/{{ page.date | date: '%Y/%m/%d' }}/index.html"
---
```

### Data available in templates

- `pkg`: The local project’s `package.json` values.

```js
{
  name: '11ty-6-min-blog',
  version: '1.0.0',
  description: '',
  main: 'index.js',
  scripts: { build: 'rm -rf _site/ && eleventy --serve' },
  author: '',
  license: 'ISC',
  devDependencies: { 'clean-css': '^5.3.2', '@11ty/eleventy': '^2.0.1' }
}
```

- `pagination`, when enabled using pagination in front matter.

- `collections`: Lists of all of your content, grouped by tags.

- `page`: Has information about the current page. See code block below for page contents. For example, page.url is useful for finding the current page in a collection. See [here]{https://www.11ty.dev/docs/data-eleventy-supplied/#page-variable} for moe information.

For a template building a normal page:

```js
{
  date: 2023-07-08T12:09:06.277Z,
  inputPath: './index.njk',
  fileSlug: '',
  filePathStem: '/index',
  outputFileExtension: 'html',
  templateSyntax: 'njk',
  url: '/',
  outputPath: '_site/index.html'
}
```

For a template using pagination:

```js
{
  url: '/country-food/3/',
  outputPath: '_site/country-food/3/index.html'
}
```

- `eleventy`: Contains Eleventy-specific data from environment variables and the Serverless plugin (if used). See [here](https://www.11ty.dev/docs/data-eleventy-supplied/#page-variable) for more.

```js
{
  version: '2.0.1',
  generator: 'Eleventy v2.0.1',
  env: {
    source: 'cli',
    runMode: 'serve',
    config: '/home/grover/projects/11ty-6-min-blog/.eleventy.js',
    root: '/home/grover/projects/11ty-6-min-blog',
    isServerless: false
  }
}
```

### Adding `body` classes form data

In this example the page class comes from 11ty supplied data, and the layout class comes from the special data key via the data cascade.

```hbs
<body class="page--{% if page.fileSlug %}{{ page.fileSlug }}{% else %}home{% endif %} layout--{{ layout }}">
```

## The data cascade

See [here](https://www.11ty.dev/docs/data-cascade/) and [here](https://benmyers.dev/blog/eleventy-data-cascade/).

### Sources of data and their priority

When the data is merged in the Eleventy Data Cascade, the order of priority for sources of data is (from highest priority to lowest, see [here](https://www.11ty.dev/docs/data-cascade/)):

1. Computed Data

2. Front Matter Data in a Template

3. Template Data Files

4. Directory Data Files (and ascending Parent Directories)

5. Front Matter Data in Layouts

6. Configuration API Global Data

7. Global Data Files

As a rule, data that is defined closer to your content will be evaluated later in the data cascade, and will have a higher precedence.

For example:

**_`my-template`_**

```yaml
---
title: This is a GOOD Blog Post
tags:
  - CSS
  - HTML
layout: my-layout.njk
---
```

**_`_includes/my-layout.njk`_**

```hbs
--- title: This is a Very BAD Blog Post author: Zach tags: - JavaScript ---

<ul>
  <li>Title = {{title}}</li>
  <li>Tags = {% for item in tags %}
    {{item}}, {% endfor %}
  </li>
  <li>Author = {{author}}</li>
  <li>Layout= {{layout}}</li>
</ul>
```

**_Results:_**

```html
<ul>
  <li>Title = This is a GOOD Blog Post</li>
  <li>Tags = JavaScript, CSS, HTML,</li>
  <li>Author = Zach</li>
  <li>Layout= my-layout.njk</li>
</ul>
```

Note, the `tags` have been concatenated, the `template` has been updated to the last in the chain, but the `title` has remains the same as the template; because template front matter has a higher priority than layout front matter. The merging of the data _uses data deep merge_, which is similar to `lodash.mergewith`. Without data deep merge the `tags` would not have been merged, and the `tags` in `my-template` would have prevailed. It is possible to turn data deep merge off, see [here](https://www.11ty.dev/docs/data-deep-merge/) for more.

### Template and directory data

If data is to be applied to a specific template, or a specific directory of templates only, it can be added as JSON or JS data files. Directory data is also applied to sub-directories.

JSON will look for the following files:

1. Content Template Front Matter Data

- merged with any Layout Front Matter Data

2. Template Data File (data is only applied to `posts/subdir/my-first-blog-post.md`)

- `posts/subdir/my-first-blog-post.11tydata.js` JavaScript Data Files

- `posts/subdir/my-first-blog-post.11tydata.json`

- `posts/subdir/my-first-blog-post.json`

3. Directory Data File (data applies to all templates in `posts/subdir/*`)

- `posts/subdir/subdir.11tydata.js` JavaScript Data Files

- `posts/subdir/subdir.11tydata.json`

- `posts/subdir/subdir.json`

4. Parent Directory Data File (data applies to all templates in `posts/**/*`, including subdirectories)

- `posts/posts.11tydata.js` JavaScript Data Files

- `posts/posts.11tydata.json`

- `posts/posts.json`

5. Global Data Files in `_data/*` (`.js` or `.json` files) available to all templates.

For example, if all block posts within a directory are be added to the same collection, and use the same layout file:

***/posts/posts.json***

```json
{
  "layout": "layouts/post.njk",
  "tags": "posts",
  "permalink": "animals/{{ title | slugify }}/"
}
```

Note, the `.11tydata.` suffix, and the base name of the folder where 11ty looks for template and directory data files can be changed in the config file. See [here](https://www.11ty.dev/docs/data-template-dir/#additional-customizations).

### Global data

This is data that every template has access to.

Adding global data to the `_data` as JSON files is a good way of providing site-wide defaults for things like navigation. While, using JS files allows data to be fetched for outside sources, or expose Node.js environment variables. (see [here](https://benmyers.dev/blog/eleventy-data-cascade/#step-1-global-data)).

Note, global data can be added to either the `_data` folder, or the config file. With the data in the config file having higher priority.

Adding global data to the config file is geared towards plugins, put can also be useful when developing coding template.

#### Adding global data via the global `_data` folder

By default the global data folder is called `_data`, and is placed inside the project's input folder, which by default is the project's root directory. Both the global data directory, and the input directory can be changed from the defaults in the config file.

All `*.json` and `module.exports` values from `*.js` files in this directory will be added into a global data object available to all templates.

**JSON example:**

**_`_data/userList.json`_**

```js
["user1", "user2"];
```

This data will be available to your templates under the `userList` key (i.e. as `{{ userList }}`) like this:

```js
{
  userList: ["user1", "user2"];
}
```

If the data file is `_data/users/userList.json`, the data will be:

```js
{
  users: {
    userList: ["user1", "user2"];
  }
}
```

**JS example:**

**_\_data/contributors.js_**

```js
const fs = require("fs");
const data = fs.readFileSync(`${process.cwd()}/.all-contributorsrc`, "utf-8");
const { contributors } = JSON.parse(data);
contributors.sort((left, right) => left.name.localeCompare(right.name));

module.exports = contributors;
```

This data is then available in template/layout files as `contributors`, front the file name `contributors.js`.

**The kebab-case folder name problem**

If there is a data file `_data/nice-insects.json`, the global data can be referenced in Liquid templates as `{{ nice-insects }}`. However, this does not work in Nunjucks.

There is a workaround [here](https://github.com/11ty/eleventy/issues/567#issuecomment-575828788).

#### Using `_data` global data in front matter

This is from [here](https://github.com/11ty/eleventy/discussions/2074).

**YAML string problem**

If there is a data file `_data/insects.json`, it can be referenced from a template file as `insects`:

```YAML
kingdom: insects
```

However, using `insects` in ths way within the front matter means that `insects` is interpreted as a string (i.e. `kingdom = "insects"`). Therefore, the following template code would not work:

```hbs
{% for animal in kingdom %}
{{animal.name}}
{% endfor %}
```

To get around this, the data files can be moved to a sub-folder, such as `_data/animals/insects.json`, then the template code can look like:

```hbs
--- kingdom: insects --- {% for animal in dynamic[kingdom] %}
<p>{{animal.name}}</p>
{% endfor %}
```

#### Adding global data in the config file

This can be used to add data, or JS functions that generate data.

```JS
module.exports = function (eleventyConfig) {
  // Values can be static:
  eleventyConfig.addGlobalData('siteUrl', 'https://pants.dev');
  // functions:
  eleventyConfig.addGlobalData("myFunction", () => new Date());
}
```

This data is then available as `myStatic` and `myFunction`.

Data can also be added to the config file as a promise, or async function.

For example:

```js
const fetch = require("node-fetch");

module.exports = function (eleventyConfig) {
  eleventyConfig.addGlobalData("brhgJSON", async () =>
    fetch("https://www.brh.org.uk/site/wp-json/wp/v2/posts/?per_page=3").then(
      (response) => response.json()
    )
  );
};
```

### Computed data

Computed data is stored in the `eleventyComputer` property.

You can put your `eleventyComputed` values anywhere in the Data Cascade: Front Matter, any Data Files (you could even make an `eleventyComputed.js` global data file if you wanted to set this for your entire site). The values in `eleventyComputed` can be used to override values set elsewhere in the data cascade.

If the computer data is derived from JS functions, it must be added by either JS front matter, or a JS data file (), since YAML and JSON don't support JS functions.

Computed data is added to the cascade just before the templates are rendered, and can not be used to modify the [special data properties](https://www.11ty.dev/docs/data-configuration/), such as, `layout`, `pagination`, `tags` etc. However, computed data can be use, or set `permalink`.

**_In a JS file_**

Any arbetory bit of JS can be used to supply an `eleventyComputed` property. Where functions are used, they will be passed a `data` attribute, containing the template's data that has already been gathered from other sources (global data, font matter, directory data files etc.), although not the [special data values](https://www.11ty.dev/docs/data-configuration/), apart from `permalink`.

It sometimes helps to declare data from elsewhere in the data cascade as a dependency within JS functions. FOr more about this, [see here](https://www.11ty.dev/docs/data-computed/#declaring-your-dependencies).

```js
module.exports = {
  eleventyComputed: {
    myTemplateString: "This is assumed to be a template string!",
    myString: (data) => "This is a string!",
    myFunction: (data) => `This is a string using ${data.someValue}.`,
    myAsyncFunction: async (data) => await someAsyncThing(),
    myPromise: (data) => {
      return new Promise((resolve) => {
        setTimeout(() => resolve("100ms DELAYED HELLO"), 100);
      });
    },
  },
};
```

**_In YAML_**

This code is for a Nunjucks template.

```YAML
---
title: My Page Title
parent: My Parent Key
eleventyComputed:
  eleventyNavigation:
    key: "{{ title }}"
    parent: "{{ parent }}"
---
```

See [here](https://www.11ty.dev/docs/data-computed/) for examples. Also see the navigation plugin example [below](#an-example-using-the-eleventycomputed-object).

### Environmental variables

These are variables that are accusable by other node applications, and can be used to define production and development environments ets.

They can be set at the command line, or inr `package.json`.

See [here](https://www.11ty.dev/docs/environment-vars/) for more.

Exposing enviromental variables, so that CSS can pe processed accordingly:

**_\_data/myProject.js_**

```js
module.exports = function () {
  return {
    environment: process.env.MY_ENVIRONMENT || "development",
  };
};
```

**_index.njk_**

```hbs
{% if myProject.environment == "production" %}
<style>{{ css | cssmin | safe }}</style>
{% else %}
<style>{{ css | safe }}</style>
{% endif %}
```

**_`package.js_**

```json
"scripts": {
  "build-dev": "rm -rf _site/ && eleventy --serve --env development",
  "build-prod": "rm -rf _site/ && eleventy --env production"
},
```

### JS data files

This applies to `*.js` files within `_data`, and `*.11data.js` files in a template directory.

Note, when using JS data files for a directory or template, an object must be exported;

```js
module.exports = {
  myDataKey: "This data is now available",
};
```

Access `myDataKey`

- Code is in a template JS data file, available as `myDataKey` within that template file.

- Code is in a directory JS data file, available as `myDataKey`in any template file within that directory.

- Code us in a global JS data file within `_data`, available as `dataFileName.myDataKey`<sup>†</sup> in any template file.

<sup>†</sup>When using JS data from the `_data` folder, it will be wrapped in an object, with the key derived from the folder name. See [above](#adding-global-data-via-the-global-_data-folder)


#### Using JS data files

Export data:

```js
module.exports = ["user1", "user2"];
```

Or,

```js
module.exports = {
  name: "grover",
  address: "Somewhere"
};
```

11ty will use the returned values from functions (and the function has access to the `eleventy` data object):

```js
module.exports = function () {
  return ["user1", "user2"];
};
```

Or,

```js
module.exports = (data) => {
  return {
    name: "grover",
    address: "Somewhere",
    version: data.eleventy.version
  };
}
```

11ty automatically uses `await` for promises and `async` functions:

```js
module.exports = function () {
  return new Promise((resolve, reject) => {
    resolve(["user1", "user2"]);
  });
};
```

This example uses `fetch`:

```js
module.exports = async function () {
  const data = await fetch(
    "https://www.brh.org.uk/site/wp-json/wp/v2/posts/?per_page=3"
  )
    .then((response) => response.json())
    .then((json) => json);

  return { brhgPosts: data };
};
```

**Note, there is a [plugin](https://www.11ty.dev/docs/plugins/fetch/) to handle fetch from remote APIs. However, since 2022, Node.js has support for `fetch()`, therefore, the advantage of using the plugin is not clear.**

Global data already processed by the Config API, and the `eleventy` global variable.

```js
module.exports = function (configData) {
  if (configData.eleventy.env.source === "cli") {
    return "I am on the command line";
  }

  return "I am running programmatically via a script";
};
```

#### Data priority and JS data files

**Remember that front matter has a higher priority that global, directory, or template data files. Therefore, if the same key is used in a data file, as in the front matter, the front matter will win.**

***`blog/blog.11tydata.js`***

```js
module.exports = (data) => {
  return {
    tags: ['blog'],
    pants: 'Whitey Tighty',
    title: 'All the same',
    permalink: '/blog/{{ title | slugify }}/',
  };
};
```

***`blog/post-1.md`***

```yaml
---
tags: ['news']
title: Pants are amongst us
---
```

Following the default behaviour, `tags` will be merged, and the front matter `title` will win out over the data file `title`.

#### Data available to JS data files

JS data files have access to the `eleventy` data object, as long as they export a function.

***_data/test.js***

```js
module.exports = (data) => {
  console.log(data);
  return {};
};
```

Results in:

```js
{
  eleventy: {
    version: '2.0.1',
    generator: 'Eleventy v2.0.1',
    env: {
      source: 'cli',
      runMode: 'serve',
      config: '/home/grover/projects/11ty-6-min-blog/.eleventy.js',
      root: '/home/grover/projects/11ty-6-min-blog',
      isServerless: false
    }
  }
}
```

Template and directory JS data files have access to 

***_data/test.js***

### Custom data file formats

The default data file formats are JS and JSON. It is possible to add other formats, such as YAML and TOML, see [here](https://www.11ty.dev/docs/data-custom/) for details.

#### `addDataExtension`

```js
// Receives file contents, return parsed data
eleventyConfig.addDataExtension("fileExtension", (contents, filePath) => {
  return {};
});
```

- `fileExtensions`—A comma separated list of file extensions for the file types you what to process as data.

Or, use with options.

```js
// or with options (new in 2.0)
eleventyConfig.addDataExtension("fileExtension", {
  parser: (contents, filePath) => ({}),

  // defaults are shown:
  read: true,
  encoding: "utf8",
});
```

- `parser`—The callback function to process the file.

- `read`—When set to `false`, the first argument of the `parser` function becomes `filePath`, instead of `content`.

- `encoding`—The default id `utf8`.

#### Add YAML

To be able to process whole files of YAML as data, use the [js-yaml](https://www.npmjs.com/package/js-yaml) package.

```js
const yaml = require("js-yaml");

module.exports = (eleventyConfig) => {
  eleventyConfig.addDataExtension("yaml", (contents) => yaml.load(contents));
};
```

#### Add EXIF data

This uses the [exifr](https://www.npmjs.com/package/exifr) package to read image EXIF data.

```js
const exifr = require("exifr");

module.exports = function (eleventyConfig) {
  eleventyConfig.addDataExtension("png,jpeg,jpg", {
    parser: async (file) => {
      let exif = await exifr.parse(file, true);
      return {
        exif,
      };
    },
    read: false,
  });
};
```

The image files can be in the locations that you would normally expect to find data files, e.g. in template directories, and `_data` etc. Note, however, that the file naming convention for template and directory data files still holds, therefore, the image for `my-blog-post.md` would need to be called `my-blog-post.jpeg`.

**_Template specific data_**

Given `my-blog-post.md` and `my-blog-post.jpeg` then `exif` will be available for use in `my-blog-post.md`, and the layout file that processes it as `{{ exif | log }}`.

**_Global data_**

Given `_data/images/custom.jpeg` then `images.custom.exif` will be available for use in any template as {{ images.custom.exif | log }}.

## Filters

Filters can be used by JavaScript , Markdown, Nunjucks, Liquid, Handlebars, and WebC template. Markdown files are preprocessed as Liquid templates (by default, [the template engine can be changed](https://www.11ty.dev/docs/config/#default-template-engine-for-markdown-files)), and therefore filters can be used in Markdown files too.

### Built-in filters

[`url`](https://www.11ty.dev/docs/filters/url/): Normalize absolute paths in your content, allows easily changing deploy subdirectories for your project.

[`slugify`](https://www.11ty.dev/docs/filters/slugify/): "My string" to "my-string" for permalinks.

[`log`](https://www.11ty.dev/docs/filters/log/): console.log inside templates. Can be chained.

`get*CollectionItem`: Get next or previous collection items for easy linking using: `getPreviousCollectionItem` and `getNextCollectionItem`

### Add filters

Custom filters can be added in the config file.

```js
module.exports = function(eleventyConfig) {
  eleventyConfig.addFilter("myFunctionName", function(value) { … });
};
```

Filters can also be added for individual template engines, using:

- [`addJavaScriptFunction`](../11ty.dev/docs/languages/javascript#filters)

- [`addLiquidFilter`](https://www.11ty.dev/docs/languages/liquid/#filters)

- [`addNunjucksFilter`](https://www.11ty.dev/docs/languages/nunjucks/#filters)

- [`addHandlebarsHelper`](https://www.11ty.dev/docs/languages/handlebars/#filters)

For example:

```js
module.exports = function(eleventyConfig) {
  // Nunjucks Filter
  eleventyConfig.addNunjucksFilter("myNjkFilter", function(value) { … });
};
```

### Using filters in template (Nunjucks)

The first argument of the filter callback function is the value being filtered, subsequent parameters can be passed with the function call.

**_eleventy.js_**

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addFilter("allCaps", function (value, extra) {
    return value.toUpperCase() + extra;
  });
};
```

**_my-template.nks_**

```hbs
<p>{{ "pants" | allCaps("nipples") }}</p>
```

Here, `pants` get passed as `value`, and `nipples` as `extra`.

### Access filters in the config file

```js
module.exports = function (eleventyConfig) {
  eleventyConfig.addShortcode("myCustomImage", function (url, alt) {
    return `<img src="${eleventyConfig.getFilter("url")(url)}" alt="${alt}">`;
  });
};
```

### Async filters

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

### 11ty data in filters

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

## Shortcodes

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

- [JavaScript](https://www.11ty.dev/docs/languages/javascript/#javascript-template-functions) `*.11ty.js` (with async support). Also `addJavaScriptFunction`

- [Liquid](https://www.11ty.dev/docs/languages/liquid/#shortcodes) `*.liquid` (with async support). Also `addLiquidShortcode`.

- [Nunjucks](https://www.11ty.dev/docs/languages/nunjucks/#shortcodes) `*.njk` (with async support). Also `addNunjucksShortcode`.

- [Handlebars](https://www.11ty.dev/docs/languages/handlebars/#shortcodes) `*.hbs` (no async support). Also `addHandlebarsHelper`.

### Paired shortcodes

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

### 11ty data in shortcodes

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

### Events

Code can be run before, or after the build process.

`eleventy.before`—Runs before each standalone build, and before each time the build process starts with `--serve` and `--watch`.

`eleventy.after`—Runs after each standalone build, and after each time the build process starts with `--serve` and `--watch`.

Event arguments—An object passed to the `eleventy.before` and `eleventy.after` containing metadata about the build. See [here](https://www.11ty.dev/docs/events/#event-arguments).

`eleventy.beforeWatch`—Only run before an updating build when `--serve`, or `--watch` are used, but it does not run on the initial build, nor a standalone build.

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

## Markdown files

### Options

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

### mark-it plugins

11ty supports adding [markdown-it](https://www.npmjs.com/search?q=keywords:markdown-it-plugin) plugins, that can add features to MD processing. Plugins include adding classes, header anchors and wiki text.

See [here](https://www.11ty.dev/docs/languages/markdown/#add-your-own-plugins) for installation instructions.

For an example of using a plugin to add classes and container elements to MD files, see [here](https://davidea.st/articles/11ty-tips-i-wish-i-knew-from-the-start/#4-supercharge-markdown-with-containers-and-classes).

### Markdown in shortcodes

Markdown can be returned in shortcode, by not when it is wrapped in HTML. See [here](https://www.11ty.dev/docs/languages/markdown/#why-cant-i-return-markdown-from-paired-shortcodes-to-use-in-a-markdown-file) for details.

## Plugins: Image

This plugin uses [Sharp](https://sharp.pixelplumbing.com/), and the [11ty Fetch plugin](https://www.11ty.dev/docs/plugins/fetch/).

The plugin works best with async code, therefore, to use it in the project's template files, Nunjucks, Liquid, JavaScript, or WebC templates are needed.

**_.eleventy.js_**

```js
const Image = require("@11ty/eleventy-img");

(async () => {
  let url = "https://images.unsplash.com/photo-1608178398319-48f814d0750c";
  let stats = await Image(url, {
    widths: [300],
  });

  console.log(stats);
})();
```

- If the first argument is a full URL (not a local file path), the full sized original is downloaded and cached locally using the Fetch plugin. The cached file is stored in a directory called `.cache`. Accepted input image types are: `jpeg`, `png`, `webp`, `gif`, `tiff`, `avif`, and `svg`.

- The second argument is an options object.

- From that cached full-size original, images are created for each format and width.

- A metadata object is returned, describing those new images. It looks like:

```js
{
  webp: [
    {
      format: 'webp',
      width: 300,
      height: 300,
      url: '/img/yL0QoCVMHj-300.webp',
      sourceType: 'image/webp',
      srcset: '/img/yL0QoCVMHj-300.webp 300w',
      filename: 'yL0QoCVMHj-300.webp',
      outputPath: 'img/yL0QoCVMHj-300.webp',
      size: 10768
    }
  ],
  jpeg: [
    {
      format: 'jpeg',
      width: 300,
      height: 300,
      url: '/img/yL0QoCVMHj-300.jpeg',
      sourceType: 'image/jpeg',
      srcset: '/img/yL0QoCVMHj-300.jpeg 300w',
      filename: 'yL0QoCVMHj-300.jpeg',
      outputPath: 'img/yL0QoCVMHj-300.jpeg',
      size: 16097
    }
  ]
}
```

### Options

The full list of options, with defaults, is:

- `widths: ["auto"]`—An array of sizes, or `auto`. `auto` keeps the original with. `[500, "auto"]` makes one image of 500px width, and a second with the original width.

- `formats: ["webp", "jpeg"]`—Array of file format extensions (`jpeg`, `png`, `webp`, `avif`, and `svg` (requires SVG input)) or `auto` (for the original file format).

- `urlPath: "/img/"`—The URLs in `img` tag's `src` attribute are prefixed with this.

- `outputDir: "./img/"`—The path to the desired output directory. The default is the `./img/` folder in the project's root. To write the the project's output folder, use `./_site/img/`.

- `svgShortCircuit: false`—Skip raster formats if SVG is available.

- `svgAllowUpscale: true`—Allow SVG to upscale beyond supplied dimensions? Other file types will not be scale larger than the original file.

- `hashLength: 10`—The file name hash length. That is, the filenames are generated from hash functions, the default is to use a 10 letter long filename.

- `filenameFormat: function() {}`—Custom file name callback. For a full list of attributes passed to the callback see [here](https://www.11ty.dev/docs/plugins/image/#custom-filenames).

- `urlFormat: function() {}`—Custom full URLs

- `cacheOptions: {}`—Advanced options passed to eleventy-fetch.

- Advanced options passed to sharp
  - `sharpOptions: {}`
  - `sharpWebpOptions: {}`
  - `sharpPngOptions: {}`
  - `sharpJpegOptions: {}`
  - `sharpAvifOptions: {}`

### Use

**_.eleventy.js_**

It is `Image.generateHTML(metadata, imageAttributes)` that generates the HTML. HOwever, you can return your own markup. See [here](https://www.11ty.dev/docs/plugins/image/#make-your-own-markup) for an example.

```js
const Image = require("@11ty/eleventy-img");

module.exports = function (eleventyConfig) {
  eleventyConfig.addShortcode("image", async function (src, alt, sizes) {
    let metadata = await Image(src, {
      widths: [300, 600],
      formats: ["avif", "jpeg"],
      outputDir: "./_site/img/",
    });

    let imageAttributes = {
      alt,
      sizes,
      loading: "lazy",
      decoding: "async",
    };

    // You bet we throw an error on a missing alt (alt="" works okay)
    return Image.generateHTML(metadata, imageAttributes);
  });
};
```

**_Nunjucks template file_**

{% image "https://images.unsplash.com/photo-1608178398319-48f814d0750c", "photo of my tabby cat", "(min-width: 30em) 50vw, 100vw" %}

```html
<picture>
  <source
    type="image/avif"
    srcset="/img/yL0QoCVMHj-300.avif 300w, /img/yL0QoCVMHj-600.avif 600w"
    sizes="(min-width: 30em) 50vw, 100vw" />
  <source
    type="image/jpeg"
    srcset="/img/yL0QoCVMHj-300.jpeg 300w, /img/yL0QoCVMHj-600.jpeg 600w"
    sizes="(min-width: 30em) 50vw, 100vw" />
  <img
    alt="photo of my tabby cat"
    loading="lazy"
    decoding="async"
    src="/img/yL0QoCVMHj-300.jpeg"
    width="600"
    height="601" />
</picture>
```

### Inserting images from data

This is similar to using image files as data sources, see [here](#custom-data-file-formats).

```js
const Image = require("@11ty/eleventy-img");
const path = require("path");

module.exports = function (eleventyConfig) {
  eleventyConfig.addDataExtension("png,jpeg", {
    read: false, // Don’t read the input file, argument is now a file path
    parser: async (imagePath) => {
      let stats = await Image(imagePath, {
        widths: ["auto"],
        formats: ["avif", "webp", "jpeg"],
        outputDir: path.join(eleventyConfig.dir.output, "img", "built"),
      });

      return {
        image: {
          stats,
        },
      };
    },
  });

  // This works sync or async: images were processed ahead of time in the data cascade
  eleventyConfig.addShortcode("dataCascadeImage", (stats, alt, sizes) => {
    let imageAttributes = {
      alt,
      sizes,
      loading: "lazy",
      decoding: "async",
    };
    return Image.generateHTML(stats, imageAttributes);
  });
};
```

With a template `my-blog-post.md` and an image file `my-blog-post.jpeg`, you could use the above configuration code in your template like this:

```hbs
{% dataCascadeImage image.stats, "My alt text" %}
```

If the image is in `_data/blogImages`, then the data containing the stats are available at `blogImages.images.stats`.

## Navigation plugin

### Simple usage

```bash
npm install @11ty/eleventy-navigation --save-dev
```

**_.eleventy.js_**

```js
const eleventyNavigationPlugin = require("@11ty/eleventy-navigation");

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(eleventyNavigationPlugin);
};
```

**_Template files_**

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

**_Render_**

This code is for Nunjucks, but Liquid can also be used.

```hbs
Render as HTML
{{ collections.all | eleventyNavigation | eleventyNavigationToHtml | safe }}

Render as Markdown
{{ collections.all | eleventyNavigation | eleventyNavigationToMarkdown | safe }}
```

Both `eleventyNavigationToMarkdown` and `eleventyNavigationToHtml` will render complete markup for an infinite number of levels.

### Advanced rendering

#### `eleventyNavigation` filter

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

#### `eleventyNavigationBreadcrumb` filter

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

#### Using `eleventyNavigationToHtml` with options

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

**Example, add excerpts**

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

#### Render with custom HTML

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

#### An example using the `eleventyComputed` object

This method can be used to setup navigation data for a whole directory in a single file, rather tha the font matter of each individual template file.

If there are page templates contained in the folder `/animals/`, the `eleventyComputed` object can be set in the directory data file `/animals/animals.11tydata.js` file. If you wanted to compute this data for all template files, it could be computed in a global data file.

**_/posts/posts.11tydata.js_**

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

## Blog taxonomy

1. Create a page for each term. This example uses Nunjucks.

```hbs
--- pagination: data: collections size: 1 alias: tag filter: - tech - posts
permalink: /tags/{{tag}}/ ---
<h1>Tagged “{{tag}}”</h1>

<ol>
  {% set taglist = collections[ tag ] %} {% for post in taglist | reverse %}
  <li><a href="{{post.url}}">{{post.data.title}}</a></li>
  {% endfor %}
</ol>
```

Note, the `filter` array contains the tags used in the site that we do not which to include in the taxonomy.

2. Add `tags` to the posts.

```yaml
---
title: My First Post
tags:
  - tech
---
```

The pagination in part 1 will create `/tags/personal/index.html`.

## 404 page

See [here](https://www.11ty.dev/docs/quicktips/not-found/)

## Webc plugin

The Webc plugin is an official plugin that provides the ability to make web components within 11ty.

### Installation and basic use

```bash
npm install @11ty/eleventy-plugin-webc
```

**_.vscode/my-workspace.code-workspace_**

```json
"files.associations": {
  "*.webc": "html",
},
```

Once installed there are three basic steps:

1. Register the plugin, and let 11ty know here the components are to be found.

2. Create a component file at that location.

3. Use the component in a template file.

4. **_.eleventy.js_**

Make VSCode treat `.webc` files as HTML.

```js
const pluginWebc = require("@11ty/eleventy-plugin-webc");

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(pluginWebc, {
    components: "src/_includes/webc/*.webc",
  });
};
```

This tells 11ty where you are keeping you components.

2. Then create the file **_src/\_includes/webc/my-component.webc_**.

```html
<p>Hello everybody.</p>
```

3. And use the component in a template file, also with the `.webc` extension.

**_src/page.webc_**

```html
<my-component></my-component>
```

This will render as:

```html
<p>Hello everybody.</p>
```

**Note, when there are no styles, or scripts in the component's file, 11ty renders the component by replacing the custom element (in this case `<my-component>`), with the contents of the component's file.**

### Adding style to a component

When styles are added to a component, 11ty will no longer replace the custom element with the contents of the component.

**_src/\_includes/webc/my-component.webc_**

```html
<p>Hello, I am wearing pants.</p>
<style>
  body {
    font-weight: bold;
  }
</style>
```

Renders:

```html
<my-component>
  <p>Hello, I am wearing pants.</p>
</my-component>
```

This is because 11ty is using bundler mode to bundle the styles and scripts together.

We now have to introduce a layout file, into which the HTML boilerplate can be added.

**_src/page.webc_**

This file now only contains the component and a YAML header specifying the layout file.

```hbs
--- layout: layout.webc ---

<my-component></my-component>
```

**_src/\_include/layout.webc_**

```html

```

## SEO

### Site meta data

Put the site meta data in `_data/meta.jsonS`.

```js
// Located in _data/meta.js
module.exports = {
  // NOTE: `process.env.URL` is provided by Netlify, and may need
  // adjusted pending your host
  url: process.env.URL || "http://localhost:8080",
  siteName: "",
  siteDescription: "",
  authorName: "",
  twitterUsername: "", // no `@`
};
```

Reference the value in templates with:

```hbs
{{meta.siteName}}
```

## Render plugin

The documentation is [here](https://www.11ty.dev/docs/plugins/render/).

This plugin adds two async shortcodes, `renderTemplate` and `renderFile`, to Nunjucks, Liquid and 11ty.js templates/layouts.

The Render plugin is a built-in plugin that is included automatically. However, to use it, it must be registered in the config file.

```js
const { EleventyRenderPlugin } = require("@11ty/eleventy");

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(EleventyRenderPlugin);
};
```

#### `renderTemplate`

The `renderTemplate` shortcode allows regions of a template to be rendered using a different templating engine to the rest of the file.

For example the following will render the shortcode contents as Markdown.

```hbs
{% renderTemplate "md" %} # I am a title * I am a list * I am a list {%
endrenderTemplate %}
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

#### `renderFile`

The `renderFile` shortcode can be used to render a template file from within another template/layout file. Note, the front matter in the file being rendered will be ignored.

The template engine used to render the file is chosen using the filename extension.

```njk
{%renderFile "src/_includes/render-me.md"%}
```

Like `renderTemplate`, `eleventy` and `page` variables are available inside the template, other data can be passed in using the second argument.

`renderFile` also has a third argument to override the template engine used to process the file. For example a Nunjucks could be used to process a Markdown file.

```hbs
--- myData: key: value --- {% renderFile "./_includes/blogpost.md", myData,
"njk" %}
```

##
