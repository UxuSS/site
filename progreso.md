----
layout: page
title: Progress
language: en
permalink: /progreso/
---

{% include head.html %}
{% include header.html %}

{% assign reporting_data = site.data[page.language].reporting | default: site.data.reporting %}

<div id="main-content" class="container reportingstatus" role="main">
  <!-- Introducción -->
  <div>
    {% include components/reporting-status-introduction.html %}
  </div>

  <!-- Reporte general -->
  <div>
    {%- assign overall = reporting_data.overall -%}
    {%- assign indicators_plural = page.t.general.indicators | downcase -%}

    {%- assign status_types = site.reporting_status.status_types -%}
    {%- if site.reporting_status.status_types -%}
        {%- assign status_types = site.reporting_status.status_types -%}
    {%- endif -%}

    <div class="goal goal-overall">
        {% unless false %}
        <div class="frame goal-tiles">
            <img src="{{ site.baseurl }}/assets/img/SDG Wheel_Transparent-01.png" alt="{{ page.t.general.sdg }}" width="100" height="100" class="goal-icon-image goal-icon-image-{{ site.goal_image_extension }}"/>
        </div>
        {% endunless %}
        <div class="details">
            <h2 class="status-goal">
                {{ page.t.status.overall_reporting_status }}
            </h2>
            <span class="total"><span>{{ overall.totals.total }}</span> {{ indicators_plural }}</span>
            {% if overall.totals.total > 0 %}
            <div class="summary">
                <div class="statuses">
                {%- for status_type in status_types -%}
                {% assign status_stats = overall.statuses | where: "status", status_type.value | first %}
                {% if status_stats %}
                <div>
                    <span class="status {{ status_type.value | slugify }}"><span class="status-inner">{{ status_stats.count }}</span></span><strong>{{ status_type.label | t }}</strong><span class="value">{{ status_stats.percentage | round }}%</span>
                </div>
                {% endif %}
                {%- endfor -%}

                <br style="clear:both;">
                </div>
            </div>
            <div class="goal-stats">
                {%- for status_type in status_types -%}
                {% assign status_stats = overall.statuses | where: "status", status_type.value | first %}
                {% if status_stats %}
                {% assign status_count_precise = status_stats.count | times: 1.0 %}
                {% assign overall_total_precise = overall.totals.total  | times: 1.0 %}
                {% assign percentage_precise = status_count_precise | divided_by: overall_total_precise | times: 100 %}
                <span class="{{ status_type.value | slugify }}" style="width:{{ percentage_precise }}%" title="{{ status_type.label | t }}: {{ status_stats.percentage | round }}%"></span>
                {% endif %}
                {%- endfor -%}
            </div>
            {% endif %}
        </div>
        <br style="clear:both;">
    </div>
  </div>

  <!-- Reporte por objetivo -->
  <div>
    {%- assign indicators_plural = page.t.general.indicators | downcase -%}

    {%- assign status_types = site.reporting_status.status_types -%}
    {%- if site.reporting_status.status_types -%}
        {%- assign status_types = site.reporting_status.status_types -%}
    {%- endif -%}

    <h2>{{ page.t.status.status_by_goal }}</h2>
    {%- for goalreport in reporting_data.goals -%}
    {%- assign goal = goalreport.goal | strip | sdg_lookup -%}
    <div class="goal reporting-status-item">
        <div class="frame goal-tiles">
            <a href="{{ goal.url }}" title="{{ page.t.goal.indicators_for_goal }} {{ goal.number }}" aria-label="{{ page.t.goal.indicators_for_goal }} {{ goal.number }}">
            <img src="{{ goal.icon }}" alt="{{ goal.short | escape }} - {{ page.t.general.goal }} {{ goal.number }}" width="100" height="100" class="goal-icon-{{ goal.number }} goal-icon-image goal-icon-image-{{ site.goal_image_extension }}"/>
            </a>
        </div>
        <div class="details">
            <h3 class="status-goal">
                <a href="{{ goal.url }}">{{ goal.short }}</a>
            </h3>
            <span class="total">{{ goalreport.totals.total }} {{ indicators_plural }}</span>
            {% if goalreport.totals.total > 0 %}
            <div class="summary">
                <div class="statuses">
                    {%- for status_type in status_types -%}
                    {% assign status_stats = goalreport.statuses | where: "status", status_type.value | first %}
                    {% if status_stats %}
                    <div>
                        <span class="status {{ status_type.value | slugify }}"><span class="status-inner">{{ status_stats.count }}</span></span><strong>{{ status_type.label | t }}</strong><span class="value">{{ status_stats.percentage | round }}%</span>
                    </div>
                    {% endif %}
                    {%- endfor -%}
                    <br style="clear:both;">
                </div>
            </div>
            <div class="goal-stats">
                {%- for status_type in status_types -%}
                    {% assign status_stats = goalreport.statuses | where: "status", status_type.value | first %}
                    {% if status_stats %}
                    {% assign status_count_precise = status_stats.count | times: 1.0 %}
                    {% assign goalreport_total_precise = goalreport.totals.total  | times: 1.0 %}
                    {% assign percentage_precise = status_count_precise | divided_by: goalreport_total_precise | times: 100 %}
                    <span class="{{ status_type.value | slugify }}" style="width:{{ percentage_precise }}%" title="{{ status_type.label | t }}: {{ status_stats.percentage | round }}%"></span>
                    {% endif %}
                {%- endfor -%}
            </div>
            {% endif %}
            <div class="divider"></div>
        </div>
        <br style="clear:both;">
    </div>
    {%- endfor -%}
  </div>

  {% include back-to-top.html %}
</div>
{% include footer.html %}

