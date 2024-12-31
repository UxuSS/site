---
title: Municipal
language: en
permalink: municipal/
layout: page
---

## Lista de Indicadores

<div style="max-width: 800px; margin: 0 auto; padding: 20px;">
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

    <div style="border-bottom: 1px solid #ccc; margin-bottom: 20px; padding-bottom: 20px;">
        <!-- Icono y Título del Objetivo -->
        <div style="display: flex; align-items: center; margin-bottom: 15px;">
            <!-- Icono -->
            {% if goal_details.icon %}
            <a href="{{ goal_details.url }}" style="margin-right: 15px;">
                <img src="{{ goal_details.icon }}" alt="{{ goal_details.short | escape }}" width="80" height="80" style="display: block; border-radius: 8px;">
            </a>
            {% endif %}
            
            <!-- Título -->
            <h3 style="margin: 0; font-size: 1.5em; font-weight: bold;">
                {% if goal_details.short %}
                <a href="{{ goal_details.url }}" style="text-decoration: none; color: #333;">{{ goal_details.short }}</a>
                {% else %}
                Objetivo {{ goal_number }}
                {% endif %}
            </h3>
        </div>

        <!-- Listado de Indicadores -->
        <ul style="list-style: none; padding: 0; margin: 10px 0 0;">
          {% for indicator_row in indicators %}
            {% assign indicator_number = indicator_row.Indicator | remove: "#" %}
            {% assign indicator_goal = indicator_number | split: "-" | first %}
            {% if indicator_goal == goal_number %}
              {% assign indicator = indicator_number | sdg_lookup %}
              {% if indicator %}
                <li style="margin-bottom: 8px; padding: 5px 0; border-bottom: 1px solid #eaeaea;">
                  <strong style="color: #0056b3;">{{ indicator.number }}</strong>: 
                  <a href="{{ indicator.url }}" style="text-decoration: none; color: #0056b3;">{{ indicator.name }}</a>
                </li>
              {% endif %}
            {% endif %}
          {% endfor %}
        </ul>
    </div>
  {% endfor %}
</div>
