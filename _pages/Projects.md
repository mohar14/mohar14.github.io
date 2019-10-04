---
layout: archive
permalink: /Projects/
title: "Projects"
author_profile: true
header:
  image: #"/images/fort point.png"
---
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
