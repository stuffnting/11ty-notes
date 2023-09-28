# The data cascade

See [here](https://www.11ty.dev/docs/data-cascade/) and [here](https://benmyers.dev/blog/eleventy-data-cascade/).

## Sources of data and their priority

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

## Merging data

The `tags` in the example above have been concatenated, the `template` has been updated to the last in the chain, but the `title` has remains the same as the template; because template front matter has a higher priority than layout front matter. The merging of the data _uses data deep merge_, which is similar to `lodash.mergewith`. Without data deep merge the `tags` would not have been merged, and the `tags` in `my-template` would have prevailed. It is possible to turn data deep merge off, see [here](https://www.11ty.dev/docs/data-deep-merge/) for more.

## Template and directory data

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

**_`/posts/posts.json`_**

```json
{
  "layout": "layouts/post.njk",
  "tags": "posts",
  "permalink": "animals/{{ title | slugify }}/"
}
```

Note, the `.11tydata.` suffix, and the base name of the folder where 11ty looks for template and directory data files can be changed in the config file. See [here](https://www.11ty.dev/docs/data-template-dir/#additional-customizations).

## Global data

This is data that every template has access to.

Adding global data to the `_data` as JSON files is a good way of providing site-wide defaults for things like navigation. While, using JS files allows data to be fetched for outside sources, or expose Node.js environment variables. (see [here](https://benmyers.dev/blog/eleventy-data-cascade/#step-1-global-data)).

Note, global data can be added to either the `_data` folder, or the config file. With the data in the config file having higher priority.

Adding global data to the config file is geared towards plugins, put can also be useful when developing coding template.

### Adding global data via the global `_data` folder

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

**_`\_data/contributors.js`_**

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

### Using `_data` global data in front matter

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

### Adding global data in the config file

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

## Computed data

Computed data is stored in the `eleventyComputer` property.

You can put your `eleventyComputed` values anywhere in the Data Cascade: Front Matter, any Data Files (you could even make an `eleventyComputed.js` global data file if you wanted to set this for your entire site). The values in `eleventyComputed` can be used to override values set elsewhere in the data cascade.

If the computer data is derived from JS functions, it must be added by either JS front matter, or a JS data file (), since YAML and JSON don't support JS functions.

Computed data is added to the cascade just before the templates are rendered, and can not be used to modify the [special data properties](https://www.11ty.dev/docs/data-configuration/), such as, `layout`, `pagination`, `tags` etc. However, computed data can be use, or set `permalink`.

**In a JS file**

Any arbitrary bit of JS can be used to supply an `eleventyComputed` property. Where functions are used, they will be passed a `data` attribute, containing the template's data that has already been gathered from other sources (global data, font matter, directory data files etc.), although not the [special data values](https://www.11ty.dev/docs/data-configuration/), apart from `permalink`.

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

**In YAML**

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

### Accessing collections in computed data

When using `.11tydata.js` files to set `eleventyComputed` data, access to `data.collections` can be confusing.

The following logs two empty objects for `data.collections`:

```js
module.exports = {
  eleventyComputed: {
    collectionTing(data) {
      console.log(data.collections);
    },
  },
};
```

But, this logs one empty object, then a populated object:

```js
module.exports = {
  eleventyComputed: {
    async collectionTing(data) {
      await data.collections;
      console.log(data.collections);
    },
  },
};
```

And, this logs an empty array, followed by a populated array:

```js
module.exports = {
  eleventyComputed: {
    collectionTing(data) {
      console.log(data.collections.all);
    },
  },
};
```

### Global computed data: `_data/eleventyComputed.js`

As well as directory and template `.11tydata.js` files, a global `_data/eleventyComputed.js` file can be used to globally set computed data for all templates.

**_`_data/eleventyComputed.js`_**

```js
module.exports = {
  myCopyrights: (data) => {
    return data.copyrights.feed.entry;
  },
  myLinks: (data) => {
    return data.links.feed.entry;
  },
};
```

### Global computed data: `eleventyConfig.addGlobalData`

11ty computed data can be added in the config file, so that it is set globally, and overrides any previous values in the data cascade.

This example (see [here](https://www.11ty.dev/docs/quicktips/draft-posts/) for the full code) does not build a draft post, if the `draft` key is set, and if `process.env.BUILD_DRAFTS` is `false`. The callback function returned will replace any previous value of `permalink` set furtherer up the data cascade.

```js
module.exports = (eleventyConfig) => {
  eleventyConfig.addGlobalData("eleventyComputed.permalink", function () {
    return (data) => {
      // Always skip during non-watch/serve builds
      if (data.draft && !process.env.BUILD_DRAFTS) {
        return false;
      }
      return data.permalink;
    };
  });
}
```

## Environmental variables

These are variables that are accusable by other node applications, and can be used to define production and development environments ets.

They can be set at the command line, or inr `package.json`.

See [here](https://www.11ty.dev/docs/environment-vars/) for more.

Exposing enviromental variables, so that CSS can pe processed accordingly:

**_`\_data/myProject.js`_**

```js
module.exports = function () {
  return {
    environment: process.env.MY_ENVIRONMENT || "development",
  };
};
```

**_`index.njk`_**

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

## JS data files

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

### Using JS data files

Export data:

```js
module.exports = ["user1", "user2"];
```

Or,

```js
module.exports = {
  name: "grover",
  address: "Somewhere",
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
    version: data.eleventy.version,
  };
};
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

### Data priority and JS data files

**Remember that front matter has a higher priority that global, directory, or template data files. Therefore, if the same key is used in a data file, as in the front matter, the front matter will win.**

**_`blog/blog.11tydata.js`_**

```js
module.exports = (data) => {
  return {
    tags: ["blog"],
    pants: "Whitey Tighty",
    title: "All the same",
    permalink: "/blog/{{ title | slugify }}/",
  };
};
```

**_`blog/post-1.md`_**

```yaml
---
tags: ["news"]
title: Pants are amongst us
---
```

Following the default behaviour, `tags` will be merged, and the front matter `title` will win out over the data file `title`.

### Data available to JS data files

JS data files have access to the `eleventy` data object, as long as they export a function.

**_data/test.js_**

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

**data/test.js**

...

## Custom data file formats

The default data file formats are JS and JSON. It is possible to add other formats, such as YAML and TOML, see [here](https://www.11ty.dev/docs/data-custom/) for details.

### `addDataExtension`

```js
// Receives file contents, return parsed data
eleventyConfig.addDataExtension("fileExtension", (contents, filePath) => {
  return {};
});
```

**`fileExtensions`**—A comma separated list of file extensions for the file types you what to process as data.

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

**`parser`**—The callback function to process the file.

**`read`**—When set to `false`, the first argument of the `parser` function becomes `filePath`, instead of `content`.

**`encoding`**—The default id `utf8`.

### Add YAML

To be able to process whole files of YAML as data, use the [js-yaml](https://www.npmjs.com/package/js-yaml) package.

```js
const yaml = require("js-yaml");

module.exports = (eleventyConfig) => {
  eleventyConfig.addDataExtension("yaml", (contents) => yaml.load(contents));
};
```

### Add EXIF data

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

**Template specific data**

Given `my-blog-post.md` and `my-blog-post.jpeg` then `exif` will be available for use in `my-blog-post.md`, and the layout file that processes it as `{{ exif | log }}`.

**Global data**

Given `_data/images/custom.jpeg` then `images.custom.exif` will be available for use in any template as {{ images.custom.exif | log }}.
