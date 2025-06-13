---
layout: page
title: Galleries
permalink: /galleries/
---

<ul>
  {% for post in site.posts %}
    {% if post.categories contains "gallery" %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> <br>
        <small>{{ post.date | date: site.minima.date_format }}</small>
      </li>
    {% endif %}
  {% endfor %}
</ul>
