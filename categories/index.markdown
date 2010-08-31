---
layout: default
title: Categories
blurb: Categories! Wee!
---

<ul>
{% for category in site.categories %}
	<li><a href="/categories/{{ category[0] }}.html">{{ category[0] }}</a></li>
{% endfor %}
</ul>
