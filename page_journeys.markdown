---
layout: page
title: Journeys
permalink: /journeys/
---

<ul>
  {% for post in site.posts %}
    {% if post.tags contains "journey" %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a> <br>
        <small>{{ post.date | date: site.minima.date_format }}</small>
      </li>
    {% endif %}
  {% endfor %}
</ul>
