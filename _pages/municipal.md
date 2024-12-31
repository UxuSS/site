---
title: Indicadores con datos municipales
language: en
permalink: municipal/
layout: page
---

Indicadores con datos municipales, agrupados por objetivo

<div class="container">
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
  {% assign grouped_indicators = grouped_indicators | uniq | map: "to_integer" | sort %}

  <!-- Mostrar indicadores agrupados en orden de objetivos -->
  {% for goal_number in grouped_indicators %}
    {% assign goal_details = goal_number | append: "" | sdg_lookup %}

    <div class="goal reporting-status-item">
        <!-- Icono y Título del Objetivo -->
        <div style="display: flex; align-items: center; margin-bottom: 15px;">
            <!-- Icono -->
            {% if goal_details.icon %}
            <a href="{{ goal_details.url }}" title="{{ page.t.goal.goal_details }} {{ goal_details.number }}" style="margin-right: 15px;">
                <img src="{{ goal_details.icon }}" alt="{{ goal_details.short | escape }}" width="80" height="80" class="goal-icon-{{ goal_details.number }} goal-icon-image goal-icon-image-{{ site.goal_image_extension }}"/>
            </a>
            {% endif %}
            
            <!-- Título -->
            <h3 style="margin: 0;">
                {% if goal_details.short %}
                <a href="{{ goal_details.url }}">{{ goal_details.short }}</a>
                {% else %}
                Objetivo {{ goal_number }}
                {% endif %}
            </h3>
        </div>

        <!-- Listado de Indicadores -->
        <ul>
          {% for indicator_row in indicators %}
            {% assign indicator_number = indicator_row.Indicator | remove: "#" %}
            {% assign indicator_goal = indicator_number | split: "-" | first %}
            {% if indicator_goal == goal_number %}
              {% assign indicator = indicator_number | sdg_lookup %}
              {% if indicator %}
                <li>
                  <strong>{{ indicator.number }}</strong>: 
                  <a href="{{ indicator.url }}">{{ indicator.name }}</a>
                </li>
              {% endif %}
            {% endif %}
          {% endfor %}
        </ul>
    </div>
    <hr />
  {% endfor %}
</div>
