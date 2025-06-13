---
layout: page
title: Articles
permalink: /articles/
---

<ul>
  {% for post in site.posts %}
    {% if post.categories contains "article" %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> <br>
        <small>{{ post.date | date: site.minima.date_format }}</small>
      </li>
    {% endif %}
  {% endfor %}
</ul>
