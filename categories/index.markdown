---
layout: default
title: List of Categories
blurb: Posts divided into nice little categories!
---

<ul>
{% for category in site.categories %}
	<li><a href="/categories/{{ category[0] }}.html">{{ category[0] }}</a></li>
{% endfor %}
</ul>
