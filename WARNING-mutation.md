#  Mutating data

## Mutations in templates

```hbs
{% set item = collections.gallery[0] %}
{# DANGER: This changes the underlying object #}
{% set _ = item.data.update({ "title": "New Title" }) %} 
```
Instead use local variables:

```hbs
{% set displayTitle = item.data.title | upper %}
<h2>{{ displayTitle }}</h2>

```

## A note on mutation when making new collections

API methods, such as `getFilteredByTag(tagName)`, return new arrays, which can be sorted without affecting the original collections. However, the objects the arrays contain are passed my reference, and changing them will mutate the original.

If you modify a property inside an item (e.g., `item.data.title = "New Title"`), that change will reflect everywhere that item is used across your entire site.

Best practice is to use `map()` and `spread`:

```js
eleventyConfig.addCollection("myCustomGallery", function(collectionsApi) {
  return collectionsApi.getFilteredByTag("gallery").map(item => {
    return {
      ...item,
      data: { ...item.data, customTitle: "Modified Title" }
    };
  });
});
```