---
layout: page
title: Latest Posts
---
{% for post in site.posts limit:10 %}
<h2><a href="{{ post.url }}" rel="bookmark" title="Permanent link to ">{{ post.title }}</a></h2>
<span>{{ post.date | date: '%B' }} {{ post.date | date: '%e' }}, {{ post.date | date: '%Y' }}</span>
<p>{{ post.excerpt }}</p>
{% endfor %}