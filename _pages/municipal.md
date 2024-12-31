---
title: This is my example title
language: en
permalink: municipal/
layout: page
---

## Indicadores Municipales

<ul>
{% assign indicators = site.data.municipal %}

{% for indicator in indicators %}
  {% assign cleaned_indicator = indicator.Indicator | remove: "#" %}
  <li>
    <a href="https://eustat-des.github.io/site/{{ cleaned_indicator }}">Indicador {{ cleaned_indicator }}</a>
  </li>
{% endfor %}
</ul>
