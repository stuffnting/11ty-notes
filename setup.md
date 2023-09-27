# Setup

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

## Config files and options

The default config file, `defaultConfig.js`, is on [GitHub](https://github.com/11ty/eleventy/blob/master/src/defaultConfig.js).

For the document action see [here](https://www.11ty.dev/docs/config/).

Many of these options can be applied at the command line, as well as in the config file.

By default 11ty looks for the following files, the first one found is used:

- `.eleventy.js`
- `eleventy.config.js`
- `eleventy.config.cjs`

The options include:

**[`dir.input`](https://www.11ty.dev/docs/config/#input-directory)** changes the top level input directory, file, or glob. Note, if an input folder is set, the default it to have `_includes` and `_data` contained within it.

**[`dir.includes`](https://www.11ty.dev/docs/config/#directory-for-includes)** changes the folder where 11ty looks for template files. The default is `_includes`. Files in this folder are not processed as template file.

**[`dir.layouts`](<https://www.11ty.dev/docs/config/#directory-for-layouts-(optional)>)** it is possible to set a folder for layout, that is separate from the includes folder. Like the includes folder, file in this folder are not processed as template files.

**[`dir.data`](https://www.11ty.dev/docs/config/#directory-for-global-data-files)** the folder for global data files.

**[`dir.output`](https://www.11ty.dev/docs/config/#output-directory)** the output directory.

**[`markdownTemplateEngine`](https://www.11ty.dev/docs/config/#default-template-engine-for-markdown-files)** change the template engine that processes the Markdown files, the default is Liquid. The template engine is used to preprocess the Markdown files, before they are processed as Markdown. Set to `false` to process as Markdown only. The template engine can also be overridden on a per-file basis, using `templateEngineOverride`.

**[`htmlTemplateEngine`](https://www.11ty.dev/docs/config/#default-template-engine-for-html-files)** change the template engine that processes the HTML files, the default is Liquid. The template engine is used to preprocess the HTML files. Set to `false` to process as HTML only.

**[`templateFormats`](https://www.11ty.dev/docs/config/#template-formats)** an array of template types that should be processed. The default is to process `html`, `liquid`, `ejs`, `md`, `hbs`, `mustache`, `haml`, `pug`,` njk`, and `11ty.js`.

**[`eleventyConfig.setQuietMode(true)`](https://www.11ty.dev/docs/config/#enable-quiet-mode-to-reduce-console-noise)** enable quite mode, which limits the amount of information logged during the build process.

**[`pathPrefix`](https://www.11ty.dev/docs/config/#deploy-to-a-subdirectory-with-a-path-prefix)** if the site is in a subfolder on the server, this options will add the specified path-prefix to absolute URLs.

**[`htmlOutputSuffix`](https://www.11ty.dev/docs/config/#change-exception-case-suffix-for-html-files)** if the input folder is the same as the output folder, a suffix is added to the end of processed HTML files to prevent the template files being overwritten. The default is `-o`.

**[`eleventyConfig.setDataFileBaseName()`](https://www.11ty.dev/docs/config/#change-base-file-name-for-data-files)** when using directory specific data files, the default behaviour is for 11ty to look for JS and JSON file names that match the directory name. This can be changed. For example, setting `index` will make 11ty look for `index.json` and `index.11tydata.json` instead of using folder names.

**[`eleventyConfig.setDataFileSuffixes()`](https://www.11ty.dev/docs/config/#change-file-suffix-for-data-files)** when using directory and template specific data files, change the default `.11ty.` suffix.

**[`eleventyConfig.addTransform()`](https://www.11ty.dev/docs/config/#transforms)** can be used to transform a template's output. For example, format/prettify HTML files.

**[`eleventyConfig.addLinter()`](https://www.11ty.dev/docs/config/#linters)** add linters to analyse template output.

**[`eleventyConfig.dataFilterSelectors`](https://www.11ty.dev/docs/config/#data-filter-selectors)** include data from the data cascade in the output.

**[`/** @param {import("@11ty/eleventy").UserConfig} eleventyConfig */`](https://www.11ty.dev/docs/config/#type-definitions)** adding this at the top of the config file will add extra autocomplete features to some IDEs.

## Command line options

See [here](https://www.11ty.dev/docs/usage/).

There several options can can be used when building eleventy, including:

**`--formats=md,html,ejs`** choosing a subset of template types.

**`--serve`** start a web server (port 8080) and rebuild when template files are changed are saved. For dev server options see [here](https://www.11ty.dev/docs/dev-server/)

**`--watch`** rebuild when template files are changed, but does not start a web server.

**`--incremental`** only rebuild the files that have changed, not the whole site, when using `--serve`, and `--watch`. More about incremental builds [here](https://www.11ty.dev/docs/usage/incremental/).

**`--ignore-initial`** ignore the initial build when using `--serve`, and `--watch`.

**`--quite`** limit the amount of information logged during the build.

**`--dryrun`** simulate a build without writing tot he output folder.

**`--config=myeleventyconfig.js`** use a different config file, and not `.eleventy.js`.

**`--to=json`** output the site as JSON.

**`--input`** and **`--output`** use the same folder for input and output—use with care. See [here](https://www.11ty.dev/docs/usage/#using-the-same-input-and-output).

## Delete pervious files on each build

To delete the perviously built files each time the build script is started (but not pon each refresh, when watched files change), add the following script to `package.json`:

```json
"scripts": {
  "build": "rm -rf _site/ && eleventy --serve"
},
```

## Debug mode

Debug mode gives lots of details about the build process. See here for [more](https://www.11ty.dev/docs/debugging/).

## Watch and Serve

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


