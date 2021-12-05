---
layout: default
---
<ul class="post-listing">
  {% for post in site.posts %}
    {% if post.categories contains 'casual' %}
      <li>
        <h2><a href="{{ post.url }}" class="link">{{ post.title }}</a></h2>
      </li>
    {% endif %}
  {% endfor %}
</ul>
