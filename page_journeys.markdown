---
layout: page
title: Journeys
permalink: /journey/
---

<ul>
  {% for post in site.posts %}
    {% if post.categories contains "journey" %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> <br>
        <small>{{ post.date | date: site.minima.date_format }}</small>
      </li>
    {% endif %}
  {% endfor %}
</ul>
