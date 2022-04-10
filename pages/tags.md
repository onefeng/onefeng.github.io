---
layout: tags
title: 标签
description: 标签
keywords: 标签
comments: false
menu: 标签
permalink: /tags/
---

<section class="container posts-content">
{% assign sorted_categories = site.tags | sort %}
{% for category in sorted_categories %}
<h3 id="{{ category[0] }}">{{ category | first }}</h3>
<ol class="posts-list">
{% for post in category.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
<a class="posts-list-name" href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ol>
{% endfor %}
</section>
<!-- /section.content -->