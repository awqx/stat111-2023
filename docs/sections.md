---
layout: default
title: Sections
rank: 1
---
## Sections

Available section notes:

<ul>
  {% for sct in site.pages %}
    {% if sct.section  %}
  <li>
    <a href="{{ sct.path | replace:'.md','.html' }}">{{ sct.title }}</a>. <em>{{ sct.date | date_to_string }}</em>.
  </li>
  {% endif %}
  {% endfor %}
</ul>

Solutions will be posted by EOD the section is taught. 