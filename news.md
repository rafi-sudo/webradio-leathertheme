---
layout: default
title: Berita
---

# Berita Terbaru

<ul>
{% for post in site.news %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a> <br>
    <small>{{ post.date | date: "%d %B %Y" }} - {{ post.category }}</small>
  </li>
{% endfor %}
</ul>
