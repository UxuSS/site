
---
title: Municipal
language: es
permalink: /municipal/
layout: page
---


<div id="main-content" class="container" role="main">

  <div class="container g-0">
    <div class="row align-items-end">
      <div class="col">
        <h2 class="goal-page-heading">{{ page.t.general.targets_and_indicators }}</h2>
      </div>
    </div>
    <hr class="goal-page-column-header-rule">

    <div id="goals-container">
      <!-- Contenido dinámico cargado desde el CSV -->
    </div>
  </div>

  {% include back-to-top.html %}
</div>

{% include footer.html %}

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const csvUrl = "https://eustat-des.github.io/data/metadata-value--data_show_map--true.csv";

    fetch(csvUrl)
      .then((response) => {
        if (!response.ok) {
          throw new Error(`Error al cargar el CSV: ${response.statusText}`);
        }
        return response.text();
      })
      .then((csvData) => {
        const data = parseCSV(csvData);
        renderGoals(data);
      })
      .catch((error) => {
        console.error("Error al cargar los datos:", error);
      });

    function parseCSV(csvText) {
      const rows = csvText.split("\n").map((row) => row.split(","));
      const headers = rows[0];
      const data = rows.slice(1).map((row) => {
        const obj = {};
        row.forEach((value, index) => {
          obj[headers[index].trim()] = value.trim();
        });
        return obj;
      });
      return data.filter((row) => row["Indicator"]);
    }

    function renderGoals(data) {
      const goalsContainer = document.getElementById("goals-container");

      // Agrupar por objetivo y meta
      const groupedData = data.reduce((acc, item) => {
        const goalNumber = item["Indicator"].split("-")[0].replace("#", "").trim();
        const targetNumber = item["Indicator"].split("-").slice(0, 2).join("-");
        if (!acc[goalNumber]) {
          acc[goalNumber] = {};
        }
        if (!acc[goalNumber][targetNumber]) {
          acc[goalNumber][targetNumber] = [];
        }
        acc[goalNumber][targetNumber].push(item);
        return acc;
      }, {});

      // Crear contenido dinámico
      for (const [goal, targets] of Object.entries(groupedData)) {
        const goalDiv = document.createElement("div");
        goalDiv.className = "goal-item";

        const goalHeader = document.createElement("h3");
        goalHeader.textContent = `Objetivo ${goal}`;
        goalDiv.appendChild(goalHeader);

        for (const [target, indicators] of Object.entries(targets)) {
          const targetDiv = document.createElement("div");
          targetDiv.className = "target-item";

          const targetHeader = document.createElement("h4");
          targetHeader.textContent = `Meta ${target}`;
          targetDiv.appendChild(targetHeader);

          const indicatorsList = document.createElement("ul");

          indicators.forEach((indicator) => {
            const indicatorItem = document.createElement("li");
            indicatorItem.innerHTML = `<a href="${indicator.url}">${indicator.Indicator.replace(
              "#",
              ""
            )}: ${indicator.name}</a>`;
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
