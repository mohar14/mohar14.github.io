---
layout: archive
permalink: /Projects/
title: "Projects"
author_profile: true
header:
  image: #"/images/fort point.png"
---
{% for portfolio in site.posts %}
  <h2>
    {{ posts.title }}
  </h2>
{% endfor %}
