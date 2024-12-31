---
title: Municipal
language: en
permalink: municipal/
layout: page
---

## Lista de Indicadores

<div style="max-width: 800px; margin: 0 auto;">
  {% assign indicators = site.data.municipal %}
  {% assign grouped_indicators = "" | split: "," %}

  <!-- Agrupar indicadores por objetivo -->
  {% for indicator_row in indicators %}
    {% assign indicator_number = indicator_row.Indicator | remove: "#" %}
    {% if indicator_number contains "-" %}
      {% assign goal_number = indicator_number | split: "-" | first %}
      {% assign grouped_indicators = grouped_indicators | push: goal_number %}
    {% endif %}
  {% endfor %}
  {% assign grouped_indicators = grouped_indicators | uniq | sort_natural %}

  <!-- Mostrar indicadores agrupados en orden de objetivos -->
  {% for goal_number in grouped_indicators %}
    {% assign goal_details = goal_number | sdg_lookup %}

    <div style="margin-bottom: 40px;">
        <!-- Cabecera del Objetivo -->
        <div style="display: flex; align-items: center; margin-bottom: 20px;">
            <!-- Icono del Objetivo -->
            {% if goal_details.icon %}
            <a href="{{ goal_details.url }}" title="{{ page.t.goal.goal_details }} {{ goal_details.number }}" style="margin-right: 20px;">
                <img src="{{ goal_details.icon }}" alt="{{ goal_details.short | escape }}" width="80" height="80" style="display: block;">
            </a>
            {% endif %}
            
            <!-- Título del Objetivo -->
            <h3 style="margin: 0; font-size: 1.5em;">
                {% if goal_details.short %}
                <a href="{{ goal_details.url }}" style="text-decoration: none; color: inherit;">{{ goal_details.short }}</a>
                {% else %}
                Objetivo {{ goal_number }}
                {% endif %}
            </h3>
        </div>

        <!-- Listado de Indicadores -->
        <ul style="list-style: none; padding: 0; margin: 20px 0 0;">
          {% for indicator_row in indicators %}
            {% assign indicator_number = indicator_row.Indicator | remove: "#" %}
            {% assign indicator_goal = indicator_number | split: "-" | first %}
            {% if indicator_goal == goal_number %}
              {% assign indicator = indicator_number | sdg_lookup %}
              {% if indicator %}
                <li style="margin-bottom: 10px;">
                  <strong>{{ indicator.number }}</strong>: 
                  <a href="{{ indicator.url }}" style="text-decoration: none; color: #007bff;">{{ indicator.name }}</a>
                </li>
              {% endif %}
            {% endif %}
          {% endfor %}
        </ul>
    </div>
    <hr style="border: 0; border-top: 1px solid #ccc; margin: 40px 0;">
  {% endfor %}
</div>
