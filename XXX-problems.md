The following is run from `/blog/blog.11tydata.js`, where there is only one template file in `/blog/`.

**Collections**

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

And, this also logs an empty array, followed by a populated array:

```js
module.exports = {
  eleventyComputed: {
    collectionTing(data) {
      console.log(data.collections.all);
    },
  },
};
```

**Template front matter**

With `categories` set in the front matter of a template:

```js
module.exports = {
  eleventyComputed: {
    postCats: (data) => {
      const categories = data.categories;
      console.log(categories);
    },
  },
};
```

If `categories` is a string, it is logged twice.

If `categories` is an array, for example `['cats', 'dogs']` this is as logged:

```
[ <2 empty items> ]
[ 'cats', 'dogs' ]
```

Yet, using `console.log(categories[0])` logs:

```
cats
cats
```

Seems from [this](https://www.11ty.dev/docs/data-computed/#advanced-details) section of the documentation, that it should be simpler than this.
