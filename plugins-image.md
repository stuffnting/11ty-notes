# Plugins: Image****

This plugin uses [Sharp](https://sharp.pixelplumbing.com/), and the [11ty Fetch plugin](https://www.11ty.dev/docs/plugins/fetch/).

The plugin works best with async code, therefore, to use it in the project's template files, Nunjucks, Liquid, JavaScript, or WebC templates are needed.

***.eleventy.js***

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

## Options

The full list of options, with defaults, is:

**`widths: ["auto"]`**—An array of sizes, or `auto`. `auto` keeps the original with. `[500, "auto"]` makes one image of 500px width, and a second with the original width.

**`formats: ["webp", "jpeg"]`**—Array of file format extensions (`jpeg`, `png`, `webp`, `avif`, and `svg` (requires SVG input)) or `auto` (for the original file format).

**`urlPath: "/img/"`**—The URLs in `img` tag's `src` attribute are prefixed with this.

**`outputDir: "./img/"`**—The path to the desired output directory. The default is the `./img/` folder in the project's root. To write the the project's output folder, use `./_site/img/`.

**`svgShortCircuit: false`**—Skip raster formats if SVG is available.

**`svgAllowUpscale: true`**—Allow SVG to upscale beyond supplied dimensions? Other file types will not be scale larger than the original file.

**`hashLength: 10`**—The file name hash length. That is, the filenames are generated from hash functions, the default is to use a 10 letter long filename.

**`filenameFormat: function() {}`**—Custom file name callback. For a full list of attributes passed to the callback see [here](https://www.11ty.dev/docs/plugins/image/#custom-filenames).

**`urlFormat: function() {}`**—Custom full URLs

**`cacheOptions: {}`**—Advanced options passed to eleventy-fetch.

Advanced options passed to sharp
  - `sharpOptions: {}`
  - `sharpWebpOptions: {}`
  - `sharpPngOptions: {}`
  - `sharpJpegOptions: {}`
  - `sharpAvifOptions: {}`

## Use

***.eleventy.js***

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

***Nunjucks template file***

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

## Inserting images from data

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
