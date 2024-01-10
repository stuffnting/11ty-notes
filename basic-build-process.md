# Basic build process

## File type nomenclature

***Template***

A content file written in a format such as Markdown, HTML, Liquid, Nunjucks, and more, which Eleventy transforms into a page (or pages) in the built site.

***Layout***

A template which wraps around another template, typically to provide the scaffolding markup for content to sit in. These files are not processed unless they are called by another files.

***Partial***

Part of a layout.

## Default folders

See [here](https://github.com/11ty/eleventy/blob/master/src/defaultConfig.js).

| 11ty dir key | Folder       | Notes                                             |
| ------------ | ------------ | ------------------------------------------------- |
| input        | .            | Start building templates from the project's root. |
| output       | `\_site `    | Build to here.                                    |
| includes     | `\_includes` | Layout files. Not automatically built.            |
| data         | `\_data`     |                                                   |

## Default build

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

and:

```yaml
---
title: This is a New Path
permalink: permalink: "/blog/{% if title %}{{ title | slugify }}{% else %}{{path.fileSlug}}{% endif %}/"
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

## Changing how markdown and HTML files are dealt with

Markdown and HTML files are preprocessed using a template engine, by default Liquid. This can be changed globally in the [config file](#config-files-and-options).

Which template engine is used can also be overridden on a per-template basis, using `templateEngineOverride`.

Markdown files are unique in that more than one template engine can be specified, and the files wil be processed in order. Or, specify just `md` have no preprocessing.

```yaml
---
templateEngineOverride: njk,md
---
```

Setting `templateEngineOverride` to `false` on any template file will prevent it from being processed, and the file will be copied to the output folder.

## Build from JS files

11ty can also build from JS files, known as 11ty.js templates. The file names should be in form: `built-file-name.11ty.js`. And, the file should be in a location that 11ty builds from automatically, such as the project's root folder.

For example, if the file is `/about.11ty.js`, the built file will be `_site/about/index.html`.

### JS functions

Note, when using functions, it does not seem possible to set data.

```js
module.exports = function (data) {
  return `<p>${data.name}</p>`;
};
```

### JS classes

When using classes, it is possible to set data, including pagination and permalinks.

In this example, the keys of the map, which are turned into items in an array for pagination, are in the form:

```js
{slug: "category-slug", title: "Category Tile"}
```

```js
class Test {
  data() {
    return {
      layout: "layout.njk",
      pagination: {
        // 11ty can't use JS maps as pagination data.
        data: "collections.all",
        alias: "category",
        size: 1,
        before(paginationData, fullData) {
          // Ignore the original pagination data set, and process the category map's keys into an array.
          return Array.from(fullData.collections.catsAndPosts.keys());
        },
      },
      eleventyComputed: {
        title: (data) => data.category.title,
      },
      permalink: (data) => `/category/${data.category.slug}/`,
    };
  }

  render(data) {
    return `<h1>${data.category.title}</h1>`;
  }
}

module.exports = Test;
```

## Ignore files when building

See [here](https://www.11ty.dev/docs/ignores/) for details.

- Files and folders to be omitted from the build process can be put in a `.eleventyignore` file. `.eleventyignore` files can be placed in the project's root, or the input directory (if it is different from the root).
- Paths listed in your project’s `.gitignore` file are automatically ignored.
- `node_modules` is always ignored.
- The list of files and directories to be ignored are in `eleventyConfig.ignores`. Items can be added and removed from the list in the config file using: `()` and `eleventyConfig.ignores.delete()`.

***Note, the file watcher has an ignore list which is separate to the build process' watch list.***

### Process files without building

The options for ignoring files listed above will ignore the file completley. As a result the files are ingnored by the build process, and excluded from collections.

To process the contents of a template file, without it being built to the output folder (by default `_site`), use:

```yaml
---
permalink: false
---
```

### Build the file but exclude from collections

To build the file to the output folder, but exclude from all collections (including `collections.all`), use:

```yaml
---
eleventyExcludeFromCollections: true
tags: post
---
```

## Custom build file types

It is possible to build to file types other than HTML. For example, it is possible to build a JSON file from an object:

```hbs
--- permalink: "index.json" --- {% JSON.stringify(collections.all) %}
```

Or, handcraft the JSON. This example makes a JSON endpoint from a collection of beer reviews (from [here](https://gitlab.com/mikestreety-sites/ale-house-rock/-/blob/main/app/content/api/beers.json.njk?ref_type=heads)):

```hbs
---
permalink: api/beers.json
sitemapIgnore: true
---
[
	{% for beer in collections.beer %}
	{
		"number": "{{ beer.data.number }}",
		"title": "{{ beer.data.title | safe }}",
		"image": "{{ beer.data.imagePath }}",
		"thumbnail": "{{ beer.data.thumbnailPath }}",
		"brewery": "{{ beer.data.brewedBy }}",
		"rating": "{{ beer.data.rating }}",
		"date": "{{ beer.data.date | dateLong }}",
		"breweries": [
			{% for brewery in beer.data.breweries %}
			{
				"title": "{{ brewery.data.title }}",
				"slug": "{{ brewery.url }}"
			}{% if not loop.last %},{% endif %}
			{% endfor %}
		],
		"slug": "{{ beer.url }}"
	}{% if not loop.last %},{% endif %}
	{% endfor %}
]

```

Or, a text file:

```yaml
---
permalink: "/{{ page.fileSlug }}.txt"
---
Some plain text here.
```

## Passthrough copy

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

### Passthrough by file extension

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
