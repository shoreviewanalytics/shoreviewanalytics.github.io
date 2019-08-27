---
layout: single
permalink: /posts/

header:
  overlay_image: /assets/images/abstract.jpg
  overlay_filter: rgba(13, 0, 12, 0.35)
---


<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
