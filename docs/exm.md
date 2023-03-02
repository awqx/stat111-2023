---
layout: default
title: Exam materials
rank: 4
---

Practice and other relevant materials for exams will be centralized on this page.

<ul>
  {% assign exam = site.pages | sort:"exam" %}
  {% for exm in exam %}
    {% if exm.exam  %}
	  <li>
	    <a href="{{ exm.path | replace:'.md','.html' }}">{{ exm.title }}</a>.
	  </li>
	{% endif %}
  {% endfor %}
</ul>