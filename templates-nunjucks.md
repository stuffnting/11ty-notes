# Templates: Nunjucks files

In these eaxamples, the `page` data is provided by 11ty. [see](./)

## Add classes depending on where the template file originated (check substring in string)

In the following anything that was build from the `src/photos/` folder gets the class of `photo-gallery`.

```hbs
<div class='entry-content {{ " photo-gallery" if "src/photos/" in page.inputPath }}'>
```

## Add classes depending on the page slug (check string in array)

The following makes an array of page slugs, and them checks to see of the current page is in it. Pages in the array get the `page-content` class, whereas the default is `article-class`.

```hbs
{%- set nonArticleSlugs = ["", "home", "news", "history", "photos", "contact"] -%}

<div class='entry-content {{ "page-content" if page.fileSlug in nonArticleSlugs else "article-content" }}>
```

## Use the `default` filter to make a `title` tag

```hbs
<title>{{ title | default("Paker's Field")}}</title>
```

## Log variables using the log filter

```hbs
{{ myVariable | log }}
```

## Make a add classes to a `body` tag

```hbs
<body class="page--{% if page.fileSlug %}{{ page.fileSlug }}{% else %}home{% endif %} layout--{{ layout }}">
```

## Loop through a collection

```hbs
<ul>
{%- for post in collections.post -%}
  <li>{{ post.data.title }}</li>
{%- endfor -%}
</ul>
```

## Make a comma seperated list from an object

In this example, each category item in `categoryObjects` has a `slug` and a `title`.

```hbs
<p>
  <b>Categories:</b> 
  {% set comma = joiner() %}
  {% for category in categoryObjects -%}
    {{- comma() }} <a href="/{{ categoriesSlugBase }}/{{ category.slug }}/">{{ category.title }}</a>
  {%- endfor %}
</p>
```
