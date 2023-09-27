
# Pagination navigation

## An index of paginated pages when sourced from an array

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

## An index of paginated pages when sourced from an object

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

## Adding Previous, next, first and last links

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
