---
layout: single
permalink: /posts/

header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/abstract.jpg
---


<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
