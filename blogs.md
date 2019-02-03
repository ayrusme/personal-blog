---
layout: page
title: Blog Posts
permalink: /blog
---

{% for blog in site.blogs %}
  <h2>
    <a href="{{ blog.url }}">
      {{ blog.title }}
    </a>
  </h2>
{% endfor %}