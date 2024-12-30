----
layout: page
title: Municipal
language: en
permalink: /progreso/
---

{%- assign indicators_plural = page.t.general.indicators | downcase -%}

<div class="container">
    <!-- Título y descripción del progreso -->
    <div class="header">
        <h1>{{ page.title }}</h1>
        <p>{{ page.t.general.description }}</p>
    </div>

    <!-- Rueda de los ODS -->
    <div class="frame goal-tiles">
        <img 
            src="{{ site.baseurl }}/assets/img/SDG Wheel_Transparent-01.png" 
            alt="{{ page.t.general.sdg }}" 
            width="100" 
            height="100" 
            class="goal-icon-image goal-icon-image-{{ site.goal_image_extension }}" 
        />
    </div>

    <!-- Detalles por objetivo -->
    <div class="details">
        {% for goal in site.goals %}
        <div class="frame goal-tiles">
            <a 
                href="{{ goal.url }}" 
                title="{{ page.t.goal.indicators_for_goal }} {{ goal.number }}" 
                aria-label="{{ page.t.goal.indicators_for_goal }} {{ goal.number }}">
                <img 
                    src="{{ goal.icon }}" 
                    alt="{{ goal.short | escape }} - {{ page.t.general.goal }} {{ goal.number }}" 
                    width="100" 
                    height="100" 
                    class="goal-icon-{{ goal.number }} goal-icon-image goal-icon-image-{{ site.goal_image_extension }}"
                />
            </a>
        </div>
        {% endfor %}
        
    </div>
</div>

{% include back-to-top.html %}
