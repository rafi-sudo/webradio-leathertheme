---
layout: post
title: Berita
permalink: /news/
---

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
