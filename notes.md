---
layout: page
title: Notes
permalink: /notes/
---

<ul>
  {% for page in site.pages  %}
    {% if page.category == 'note' %}
      <li>
        <a href="{{ page.url }}">{{ page.title }}</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>
