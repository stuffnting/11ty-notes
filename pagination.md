# Pagination: create pages from data

See [here](https://rphunt.github.io/eleventy-walkthrough/pagination.html).

Pages can be created from data stored within JSON files, which are kept in the `_data` folder. Pages can also be made from data with a template's front matter.

The first few examples deal with making one page per data item. For having more than one data item per page, see [Multiple data items per page](#multiple-data-items-per-page).

## Automatic permalinks—JSON

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
--- layout: mylayout.nks pagination: data: possums size: 1 alias: possum ---

{{possum.name}}
is
{{possum.age}}
years old
```

- `data`—the file in `_data` to use as the source.
- `size`—how many items to put on each page created. To create a page per item, use 1. For details about having more than one item on a page see [below](#multiple-data-items-per-page).
- `alias`—a variable name used to refer to the data item currently being turned into a page. This is an alias for `pagination.items[0]` etc.

Note, the above code can be written with a conventional Nunjucks `for` loop. in Which case, the `alias` is not needed. Despite there being only one data item to display per page, a loop is used, which is similar to the way WorPress displays a single post/page.

```hbs
--- layout: mylayout.nks pagination: data: possums size: 1 alias: possum --- {%
for possum in pagination.items %}
{{possum.name}}
is
{{possum.age}}
years old. {% endfor %}
```

**Pages created**

The above code produces a folder `_site/possum-pages`, where the file `_site/possum-pages/index.html` contains the first data item ("Fluffy").

`_site/possum-pages` also contains three sub-folders, numbered 1 to 3, each of which contains an `index.html` file. These files contain the 2nd, 3rd and 4th data items, respectively.

## Automatic permalinks—YAML

The data can be supplied from the front matter.

**_`my-people.njk`_**

```hbs
--- pagination: data: testdata size: 1 testdata: - item1: name: Bob age: 22
-item2: name: Mike age: 33 - item3: name: Dave age: 44 - item4: name: Bill age:
55 --- {% for person in pagination.items %}
<li>{{person.name}} is {{person.age}} old.</li>
{% endfor %}
```

As with the previous section, `_site/my-people/index.html` contains the first person, and the there are 3 sub-folders, numbered 1 to 3, for the other people.

## Custom permalinks

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

## Multiple data items per page: `size` > 1

**_`\_data/multi-data-items.json`_**

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

**_`/country-food.njk`_**

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

## `alias` with multiple data items

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

## `alias` where each item is an object

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

## Paginating an object—keys

This time the global data file contains an object of objects, rather than an array of objects.

By default, 11ty will make a pagination array using `Object.keys()`. This array is then used to loop through the data items, and can also be used to reference the nested objects in the original data.

**_`\_data/multi-data-items.json`_**

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

**_`/country-food.njk`_**

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

## Paginating an object—values

By setting `resolve` to `values`, 11ty will use `Object.values()` to make the pagination array.

This example uses the same global data object as the last example.

**_`/country-food.njk`_**

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

**`items`**—an array of data items to be used ono the current page.

```js
items: [
  { item1: null, name: 'Bob', age: 22 },
  { item2: null, name: 'Mike', age: 33 }
],
```

**`hrefs`**—An array containing the relative paths of all pages created.

```js
hrefs: [ '/paged-data/', '/paged-data/1/' ],
```

**`href`**—An array containing the relative paths for the `previous`, `next`, `first`, and `last` pages.

```js
{
  previous: null,
  next: '/paged-data/1/',
  first: '/paged-data/',
  last: '/paged-data/1/'
}
```

**`pages`**—An array of the data for each page.

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

**`page`**—An array of the the data for the `next`, `previous`, `first`, and `last` pages.

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

### Use `pagination.before` for data 11ty doesn't want to paginate (paginate JS maps)

See [here](https://github.com/11ty/eleventy/issues/2365).

11ty will paginate arrays or objects, but not JS maps. Futhermore, it seems that the `pagination.data` property needs to be a string containing the data source (e.g. `data: "collections.all"`), and not a function.

To make 11ty paginate something it doesn't want to, set `pagination.data` to something that can be paginated, such as `collections.all`, then use the `pagination.before` method to transform the required data into a form that can be paginated.

This can be done in JS front matter, an `.lltydata.js` file, or a `.11ty.JS` template file.

#### JS front matter example

This example, the JS front matter in a layout file transforms the pagination in response to a tag set int the template file. `collections.all` is initially used as the data source, then `before` switches it for the collection corresponding to the `postTag` data set in the template file.

**_layouts/post-feed.njk_**

```js
---js
{
  layout: "layouts/base.njk",
  pagination: {
    data: "collections.all",
    alias: "posts",
    size: 2,
    reverse: true,
    before(paginationData, fullData) {
      // Ignore the original pagination data set (`collections.all`) and
      // return the specified `collections[postTag]` items.
      return fullData.collections[fullData.postTag];
    }
  }
}
---
```

**_index.njk_**

```yaml
---
layout: layouts/post-feed.njk
postTag: posts
---
```

#### `.11ty.js` template file - paginate a map

The map, `collections.catsAndPosts`, is created with `eleventy.addCollection()` in the config file.

```js
class Test {
  data() {
    return {
      layout: "layout.njk",
      pagination: {
        data: "collections.all",
        alias: "category",
        size: 1,
        before(paginationData, fullData) {
          // Ignore the original pagination data set (`collections.all`)
          return Array.from(fullData.collections.catsAndPosts.keys());
        },
      },
      permalink: (data) => `/more-cats/${data.category.title}/`,
    };
  }

  render(data) {
    return `<h1>${data.category.title}</h1>`;
  }
}

module.exports = Test;
```

## A full list of `pagination` options

From [here](https://www.11ty.dev/docs/pagination/#full-pagination-option-list).

**`data`** (String) Lodash.get path to point to the target data set.

**`size`** (Number, required)

**`alias`** (String) refer to the data set by a different name within the template. When `size` > 1, bracket notation needs to be used. See [here](https://www.11ty.dev/docs/pagination/#aliasing-to-a-different-variable).

**`generatePageOnEmptyData`** (Boolean) if target data set is empty, render first page with empty chunk [].

**`resolve: values`** ("values") if the data is an object, pagination iterates over the values instead of the keys.

**`filter`** (Array) values to remove from the paginated data.

**`reverse: true`** (Boolean) if `true`, reverse the oder of the data.

**`addAllPagesToCollections: true`** (Boolean) by default only the first page paginated from data will be added to the collection specified in `tags`.

**`before`** (Function) callback to modify, filter, or otherwise change the pagination data before pagination occurs.

**Note: when modifying data, the order of operation is:**

- The `before` callback
- `reverse: true`
- `filter` entries
