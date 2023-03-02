---
layout: default
title: Sections
rank: 1
---
## Sections

Section solutions will be posted by EOD Friday. 

Available section notes:

<ul>
  {% assign section = site.pages | sort:"section" %}
  {% for sct in section %}
    {% if sct.section  %}
  <li>
    <a href="{{ sct.path | replace:'.md','.html' }}">{{ sct.title }}</a>. <em>{{ sct.date | date_to_string }}</em>.
  </li>
  {% endif %}
  {% endfor %}
</ul>