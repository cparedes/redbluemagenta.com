---
layout: default
title: Categories
blurb: Categories! Wee!
---

<ul>
{% for category in site.categories %}
	<li><a href="/categories/{{ category }}.html">{{ category }}</a></li>
{% endfor %}
</ul>
