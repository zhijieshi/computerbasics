---
layout: default
title: Basic Concepts in Computer Science and Engineering
---

{% for t in site.topics %}

<h2>
<a href="{{ t.url | prepend: site.baseurl }}">
        {{ t.title }}
</a>
</h2>

<p class="post-excerpt">{{ t.description | truncate: 160 }}</p>

{% endfor %}
