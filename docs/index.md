{% for collection in site.collections %}
  <h2>{{ collection.label }}</h2>
  <ul>
    {% for item in site[collection.label] %}
      	<li><a href="{{ item.url | prepend: site.baseurl }}">
		{{ item.title }}
	  </a>
	</li>
    {% endfor %}
  </ul>
{% endfor %}
