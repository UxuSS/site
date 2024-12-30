---
title: Municipal
language: en
permalink: /municipal/
layout: page
---

<div id="main-content" class="container" role="main">

  <div class="container g-0">
    <div class="row align-items-end">
      <div class="col">
        <h2 class="goal-page-heading">Indicadores por Meta</h2>
      </div>
    </div>
    <hr class="goal-page-column-header-rule">

    <div id="goals-container">
      <!-- Contenido dinámico cargado desde el CSV -->
    </div>
  </div>

</div>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const csvUrl = "https://eustat-des.github.io/data/metadata-value--data_show_map--true.csv";

    fetch(csvUrl)
      .then(response => {
        if (!response.ok) {
          throw new Error(`Error al cargar el CSV: ${response.statusText}`);
        }
        return response.text();
      })
      .then(csvData => {
        const data = parseCSV(csvData);
        renderGoals(data);
      })
      .catch(error => {
        console.error("Error al cargar los datos:", error);
      });

    function parseCSV(csvText) {
      const rows = csvText.split("\n").map(row => row.split(","));
      const headers = rows[0];
      const data = rows.slice(1).map(row => {
        const obj = {};
        row.forEach((value, index) => {
          obj[headers[index].trim()] = value.trim();
        });
        return obj;
      });
      return data.filter(row => row["Indicator"]);
    }

    function renderGoals(data) {
      const goalsContainer = document.getElementById("goals-container");

      // Agrupar por objetivo y meta
      const groupedData = data.reduce((acc, item) => {
        const indicator = item["Indicator"].replace("#", "").trim();
        const [goalNumber, targetNumber] = indicator.split("-");
        if (!acc[goalNumber]) {
          acc[goalNumber] = {
            url: `https://example.com/goals/${goalNumber}`,
            name: `Objetivo ${goalNumber}`,
            icon: `/assets/icons/goal-${goalNumber}.png`,
            targets: {}
          };
        }
        if (!acc[goalNumber].targets[targetNumber]) {
          acc[goalNumber].targets[targetNumber] = [];
        }
        acc[goalNumber].targets[targetNumber].push({
          name: item["Indicator Name"] || `Indicador ${indicator}`,
          url: item["Indicator URL"] || `https://example.com/indicators/${indicator}`,
          number: indicator
        });
        return acc;
      }, {});

      // Crear contenido dinámico
      for (const [goalNumber, goal] of Object.entries(groupedData)) {
        const goalDiv = document.createElement("div");
        goalDiv.className = "goal-item";

        // Encabezado del objetivo con imagen y enlace
        const goalHeader = document.createElement("h3");
        goalHeader.innerHTML = `
          <a href="${goal.url}">
            <img src="${goal.icon}" alt="Icono del Objetivo ${goalNumber}" class="goal-icon-image">
            ${goal.name}
          </a>
        `;
        goalDiv.appendChild(goalHeader);

        // Iterar sobre las metas
        for (const [targetNumber, indicators] of Object.entries(goal.targets)) {
          const targetDiv = document.createElement("div");
          targetDiv.className = "target-item";

          const targetHeader = document.createElement("h4");
          targetHeader.textContent = `Meta ${goalNumber}-${targetNumber}`;
          targetDiv.appendChild(targetHeader);

          const indicatorsList = document.createElement("ul");

          indicators.forEach(indicator => {
            const indicatorItem = document.createElement("li");
            indicatorItem.innerHTML = `
              <span class="indicator-name">
                <a href="${indicator.url}">${indicator.number}: ${indicator.name}</a>
              </span>
            `;
            indicatorsList.appendChild(indicatorItem);
          });

          targetDiv.appendChild(indicatorsList);
          goalDiv.appendChild(targetDiv);
        }

        goalsContainer.appendChild(goalDiv);
      }
    }
  });
</script>
