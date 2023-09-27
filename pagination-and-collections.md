# Pagination and collections

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

