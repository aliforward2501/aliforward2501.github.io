---
title: 所有笔记
permalink: /posts/
---
{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
