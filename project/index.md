---
layout: page
title: Project
permalink: /project/
---

<ul>
  {% for category in site.categories %}
    <li>
      <a href="{{ site.baseurl }}/project/{{ category[0] }}">{{ category[0] }} ({{ category[1].size }})</a>
    </li>
  {% endfor %}
</ul>
