---
title: This is my example title
language: en
permalink: municipal/
layout: page
---

<div id="main-content" class="container" role="main">

  <h2>{{ page.t.general.targets_and_indicators }}</h2>

  <div class="container g-0">
    <div class="row align-items-end">
      <div class="col">
        <h2 class="goal-page-heading">{{ page.t.general.indicators }}</h2>
      </div>
    </div>
    <hr class="goal-page-column-header-rule">

    {% assign indicators = site.data.municipal %}

    {% for indicator in indicators %}
      {% assign indicator_data = indicator.indicator | remove: "#" | sdg_lookup %}
      <div class="row align-items-start goal-indicator">
        <!-- Número del Indicador -->
        <div class="col-auto goal-page-indicator-number">
          <h4>
            <span class="indicator-number">
              <span class="visually-hidden">{{ page.t.general.indicator }}</span> {{ indicator_data.number }}
            </span>
          </h4>
        </div>
        
        <!-- Título del Indicador -->
        <div class="col goal-page-indicator-name">
          <span class="indicator-name">
            <a href="{{ indicator_data.url }}">{{ indicator_data.name }}</a>
          </span>
        </div>
      </div>
      <hr class="goal-page-target-rule" />
    {% endfor %}
  </div>

</div>
