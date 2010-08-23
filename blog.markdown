---
layout: default
title: Blog
blurb: Here are my latest posts.
---

{% for post in paginator.posts %}
{% include postdetail.html %}
{% endfor %}

<div class="pageNav">
{% if paginator.next_page %}
	<p class="prevNav"><a href="/page{{ paginator.next_page }}">&larr; Older</a></p>
{% endif %}
{% if paginator.previous_page %}
	<p class="nextNav">
	{% if paginator.previous_page == 1 %}
		<a href="/blog">
	{% else %}
		<a href="/page{{ paginator.previous_page }}">
	{% endif %}
		Newer &rarr;
		</a>
	</p>
{% endif %}
</div>
