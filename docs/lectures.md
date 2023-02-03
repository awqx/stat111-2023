---
layout: default
title: Lectures
rank: 2
---
## Lecture Notes
I'll occasionally add personal notes into the lecture notes. 

Available lecture notes:
<ul>
  {% for lct in site.pages %}
    {% if lct.lecture  %}
  <li>
    <a href="{{ lct.path | replace:'.md','.html' }}">{{ lct.title }}</a>. <em>{{ lct.date | date_to_string }}</em>.
  </li>
  {% endif %}
  {% endfor %}
</ul>