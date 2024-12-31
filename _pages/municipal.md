---
title: Municipal
language: en
permalink: municipal/
layout: page
---

## Lista de Indicadores

<ul>
  {% assign indicators = site.data.municipal %}
  {% for indicator_row in indicators %}
    {% assign indicator_number = indicator_row.Indicator | remove: "#" %}
    {% assign indicator = indicator_number | sdg_lookup %}
    {% if indicator %}
      <li>
        <strong>{{ indicator.number }}</strong>: 
        <a href="{{ indicator.url }}">{{ indicator.name }}</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>
