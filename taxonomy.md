## Blog taxonomy

## Using `tags` and `pagination`

**1\. Create a page for each term. This example uses Nunjucks.**

```hbs
---
pagination: 
  data: collections
  size: 1
  alias: tag
filter:
  - tech
  - posts
permalink: /tags/{{tag}}/
---

<h1>Tagged “{{tag}}”</h1>

<ol>
  {% set taglist = collections[ tag ] %} {% for post in taglist | reverse %}
  <li><a href="{{post.url}}">{{post.data.title}}</a></li>
  {% endfor %}
</ol>
```

Note, the `filter` array contains the tags used in the site that we do not want to include in the taxonomy.

**2\. Add `tags` to the posts.**

```yaml
---
title: My First Post
tags:
  - tech
---
```

The pagination in part 1 will create `/tags/personal/index.html`.
