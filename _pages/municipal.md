---
title: This is my example title
language: en
permalink: municipal/
layout: page
---

## Lista de Indicadores

<div class="container">
  <ul>
    {% assign indicators = site.data.municipal %}
    {% for indicator in indicators %}
      {% assign indicator_data = indicator.number | sdg_lookup %}
      <li>
        <strong>{{ indicator_data.number }}</strong>: 
        <a href="{{ indicator_data.url }}">{{ indicator_data.name }}</a>
      </li>
    {% endfor %}
  </ul>
</div>
