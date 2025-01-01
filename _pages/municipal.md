---
title: Indicadores con datos municipales
language: en
permalink: municipal/
layout: page
---

Indicadores con datos municipales

<div class="container">
  {% assign indicators = site.data.municipal %}
  {% assign ordered_goals = "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17" | split: "," %}

  <!-- Mostrar indicadores agrupados en orden de objetivos -->
  {% for goal_number in ordered_goals %}
    {% assign goal_details = goal_number | sdg_lookup %}

    <div class="goal reporting-status-item">
        <!-- Cabecera del Objetivo -->
        <div style="display: flex; align-items: center; margin-bottom: 15px;">
            <!-- Icono del Objetivo -->
            {% if goal_details.icon %}
            <a href="{{ goal_details.url }}" title="{{ page.t.goal.goal_details }} {{ goal_details.number }}" style="margin-right: 15px;">
                <img src="{{ goal_details.icon }}" alt="{{ goal_details.short | escape }}" width="80" height="80" class="goal-icon-{{ goal_details.number }} goal-icon-image goal-icon-image-{{ site.goal_image_extension }}"/>
            </a>
            {% endif %}
            
            <!-- Título del Objetivo -->
            <h3 style="margin: 0;">
                {% if goal_details.short %}
                <a href="{{ goal_details.url }}">{{ goal_details.short }}</a>
                {% else %}
                Objetivo {{ goal_number }}
                {% endif %}
            </h3>
        </div>

        <!-- Listado de Indicadores -->
        <ul style="margin-top: 10px;">
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
    <hr style="border: 0; border-top: 1px solid #ccc; margin: 20px 0;">
  {% endfor %}
</div>
