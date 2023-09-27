# Templates and data

## Special front matter data keys

There are a few special data keys you can assign in your data to control how templates behave. These can live anywhere in the Data Cascade, such as front matter, and include:

**`permalink`:** Change the output target of the current template. Normally, you cannot use template syntax to reference other variables in your data, but permalink is an exception. Setting `permalink` to `false` allows the page to be processed, but prevents the page being built.

**`layout`:** Wrap current template with a layout template found in the `_includes` folder.

**`pagination`:** Enable to iterate over data. Output multiple HTML files from a single template.

**`tags`:** A single string or array that identifies that a piece of content is part of a collection. Collections can be reused in any other template.

**`date`:** Override the default date (file creation) to customize how the file is sorted in a collection.

**`templateEngineOverride`:** Override the template engine on a per-file basis.

**`eleventyExcludeFromCollections`:** Set to `true` to exclude this content from any and all Collections.

**`eleventyComputed`:** Programmatically set data values based on other values in your data cascade. When used in YAML front matter, this allows for all data values, not just `permalink`, to be set using template strings.

**`eleventyNavigation`:** Used by the Navigation plugin.

**`eleventyImport`:** An array to declare any collections used in the page. This helps with incremental builds, and template build order. See [here](https://github.com/11ty/eleventy/issues/975).

These are all available in template/layout files using their keys, e.g. `{{ date }}`. They also appear in the `data` property of `collections` objects.

**Note, only `permalink` and `eleventyComputed` can contain variable and shortcodes**

If you need to use variables or shortcodes in other front matter values, use `eleventyComputed` to set them.

## 11ty supplied data available in templates

**`pkg`:** The local project’s `package.json` values.

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

**`pagination`** when enabled using pagination in front matter.

**`collections`:** Lists of all of your content, grouped by tags.

**`page`:** Has information about the current page. See code block below for page contents. For example, page.url is useful for finding the current page in a collection. See [here]{https://www.11ty.dev/docs/data-eleventy-supplied/#page-variable} for moe information.

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

**`eleventy`:** Contains Eleventy-specific data from environment variables and the Serverless plugin (if used). See [here](https://www.11ty.dev/docs/data-eleventy-supplied/#page-variable) for more.

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

## Front matter `date`

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

## Adding `body` classes from data

In this example the page class comes from 11ty supplied data, and the layout class comes from the special data key via the data cascade.

```hbs
<body class="page--{% if page.fileSlug %}{{ page.fileSlug }}{% else %}home{% endif %} layout--{{ layout }}">
```
