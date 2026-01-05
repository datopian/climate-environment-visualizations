---
toc: false
---

```js
const globalTempRaw = FileAttachment("data/global-temp-annual.csv").csv({typed: true});
const csiroRaw = FileAttachment("data/CSIRO_Recons_gmsl_yr_2019.csv").csv({typed: true});
const glaciersRaw = FileAttachment("data/glaciers.csv").csv({typed: true});
const co2Raw = FileAttachment("data/co2-mm-mlo.csv").csv({typed: true});
```

```js
const globalTempData = (await globalTempRaw)
  .filter(d => d.Source === "GISTEMP")
  .map(d => ({ year: +d.Year, value: +d.Mean }))
  .filter(d => !isNaN(d.value) && d.year >= 1880);

const csiroData = (await csiroRaw).map(d => ({
  year: +d.Time,
  value: +d.GMSL
})).filter(d => d.year >= 1880 && !isNaN(d.value));

const glaciersData = (await glaciersRaw).map(d => ({
  year: +d.Year,
  value: +d["Mean cumulative mass balance"]
})).filter(d => !isNaN(d.value) && d.year >= 1956);

const co2Data = (await co2Raw).map(d => ({
  date: new Date(d.Date),
  year: d["Decimal Date"],
  yearInt: Math.floor(d["Decimal Date"]),
  average: +d.Average
})).filter(d => !isNaN(d.average) && d.average > 0);

const latestTemp = globalTempData[globalTempData.length - 1];
const latestSeaLevel = csiroData[csiroData.length - 1];
const latestGlacier = glaciersData[glaciersData.length - 1];
const latestCO2 = co2Data[co2Data.length - 1];
const firstCO2 = co2Data[0];

const tempChange = latestTemp.value - globalTempData[0].value;
const seaLevelChange = latestSeaLevel.value - csiroData[0].value;
const glacierChange = latestGlacier.value - glaciersData[0].value;
const co2Increase = latestCO2.average - firstCO2.average;
```



```js
display(html`<div class="dashboard-container">
  <div class="dashboard-top">
    <div class="sidebar-section">
      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-card-header">
            <span class="stat-card-label">Current CO₂</span>
          </div>
          <div class="stat-card-content">
            <div class="stat-card-value">${latestCO2.average.toFixed(0)}</div>
            <div class="stat-card-subvalue">PPM</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-card-header">
            <span class="stat-card-label">Temp Rise</span>
          </div>
          <div class="stat-card-content">
            <div class="stat-card-value" style="color: var(--color-accent-red)">+${tempChange.toFixed(2)}°C</div>
            <div class="stat-card-subvalue">Since 1880</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-card-header">
            <span class="stat-card-label">Sea Level</span>
          </div>
          <div class="stat-card-content">
            <div class="stat-card-value" style="color: var(--color-accent-blue)">+${seaLevelChange.toFixed(0)}</div>
            <div class="stat-card-subvalue">mm Rise</div>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-card-header">
            <span class="stat-card-label">Glaciers</span>
          </div>
          <div class="stat-card-content">
            <div class="stat-card-value" style="color: var(--color-accent-orange)">${glacierChange.toFixed(1)}</div>
            <div class="stat-card-subvalue">Meters (w.e.)</div>
          </div>
        </div>
      </div>

      <div class="insights-card">
        <div class="insights-header">
          <h4 class="insights-title">Key Insights</h4>
        </div>
        <ul class="insights-list">
          <li>CO₂ concentrations are at their highest level in human history (~${latestCO2.average.toFixed(0)} PPM)</li>
          <li>Global temperatures have risen ${tempChange.toFixed(2)}°C, driving extreme weather events</li>
          <li>Sea levels are rising at an accelerating rate due to thermal expansion and ice melt</li>
          <li>Glaciers are retreating globally, threatening freshwater supplies</li>
        </ul>
      </div>
    </div>

    <div class="chart-card">
      <div class="chart-header">
        <div class="chart-title-row">
          <h3 class="chart-title">Global Temperature & Sea Level Rise (1880-2019)</h3>
        </div>
      </div>
      <div class="chart-content">
        ${resize((width) => {
          const isMobile = width < 640;
          const height = isMobile ? 320 : 380;
          const marginTop = 20;
          const marginRight = isMobile ? 50 : 60;
          const marginBottom = isMobile ? 50 : 40;
          const marginLeft = isMobile ? 50 : 60;

          const container = d3.create("div").style("position", "relative");
          const svg = container.append("svg")
            .attr("width", width).attr("height", height)
            .attr("viewBox", [0, 0, width, height])
            .style("overflow", "visible");

          // Clipping path to prevent lines going off-chart
          svg.append("defs").append("clipPath")
            .attr("id", "chart-clip")
            .append("rect")
            .attr("x", marginLeft)
            .attr("y", marginTop)
            .attr("width", width - marginLeft - marginRight)
            .attr("height", height - marginBottom - marginTop);

          const xScale = d3.scaleLinear().domain([1880, 2019]).range([marginLeft, width - marginRight]);

          const yTempScale = d3.scaleLinear()
            .domain([d3.min(globalTempData, d => d.value), d3.max(globalTempData, d => d.value)])
            .range([height - marginBottom, marginTop]).nice();

          const ySeaLevelScale = d3.scaleLinear()
            .domain([d3.min(csiroData, d => d.value), d3.max(csiroData, d => d.value)])
            .range([height - marginBottom, marginTop]).nice();

          const lineTemp = d3.line().x(d => xScale(d.year)).y(d => yTempScale(d.value)).curve(d3.curveCatmullRom);
          const lineSeaLevel = d3.line().x(d => xScale(d.year)).y(d => ySeaLevelScale(d.value)).curve(d3.curveCatmullRom);

          // Grid
          svg.append("g").attr("transform", `translate(${marginLeft},0)`)
            .call(d3.axisLeft(yTempScale).tickSize(-(width - marginLeft - marginRight)).tickFormat(""))
            .call(g => g.select(".domain").remove())
            .call(g => g.selectAll(".tick line").attr("stroke", "#e5e7eb").attr("stroke-opacity", 0.5));

          // Axes
          svg.append("g").attr("transform", `translate(0,${height - marginBottom})`)
            .call(d3.axisBottom(xScale).tickFormat(d3.format("d")));

          svg.append("g").attr("transform", `translate(${marginLeft},0)`)
            .call(d3.axisLeft(yTempScale).tickFormat(d => `${d}°C`))
            .call(g => g.selectAll("text").attr("fill", "#dc2626").attr("font-weight", "bold"));

          svg.append("g").attr("transform", `translate(${width - marginRight},0)`)
            .call(d3.axisRight(ySeaLevelScale).tickFormat(d => `${d}mm`))
            .call(g => g.selectAll("text").attr("fill", "#1d4ed8").attr("font-weight", "bold"));

          // Lines with clipping
          svg.append("path")
            .datum(globalTempData)
            .attr("fill", "none")
            .attr("stroke", "#dc2626")
            .attr("stroke-width", 2.5)
            .attr("d", lineTemp)
            .attr("clip-path", "url(#chart-clip)");

          svg.append("path")
            .datum(csiroData)
            .attr("fill", "none")
            .attr("stroke", "#1d4ed8")
            .attr("stroke-width", 2.5)
            .attr("d", lineSeaLevel)
            .attr("clip-path", "url(#chart-clip)");

          // Legend
          const legend = svg.append("g").attr("transform", `translate(${marginLeft + 20}, ${marginTop + 10})`);
          legend.append("rect").attr("width", 140).attr("height", 50).attr("fill", "white").attr("fill-opacity", 0.8).attr("rx", 4);

          legend.append("line").attr("x1", 10).attr("x2", 30).attr("y1", 20).attr("y2", 20).attr("stroke", "#dc2626").attr("stroke-width", 2.5);
          legend.append("text").attr("x", 35).attr("y", 24).text("Temperature").attr("font-size", "11px").attr("font-weight", "600");

          legend.append("line").attr("x1", 10).attr("x2", 30).attr("y1", 38).attr("y2", 38).attr("stroke", "#1d4ed8").attr("stroke-width", 2.5);
          legend.append("text").attr("x", 35).attr("y", 42).text("Sea Level").attr("font-size", "11px").attr("font-weight", "600");

          return container.node();
        })}
      </div>
    </div>
  </div>

  <div class="dashboard-bottom" style="grid-template-columns: 1fr 1fr;">
    <div class="chart-card">
      <div class="chart-header"><h3 class="chart-title">The Keeling Curve (CO₂)</h3></div>
      <div class="chart-content">
        ${resize((width) => Plot.plot({
          width, height: 300, marginLeft: 50, marginBottom: 40,
          y: { grid: true, label: "CO₂ (PPM)", domain: [300, 430] },
          marks: [
            Plot.areaY(co2Data, {x: "date", y: "average", fill: "#0ea5e9", fillOpacity: 0.1}),
            Plot.line(co2Data, {x: "date", y: "average", stroke: "#0ea5e9", strokeWidth: 2}),
            Plot.ruleY([350], {stroke: "#f59e0b", strokeDasharray: "4,4"}),
            Plot.text(["Safe Limit (350)"], {x: d3.max(co2Data, d => d.date), y: 350, dy: -6, textAnchor: "end", fill: "#f59e0b"})
          ]
        }))}
      </div>
    </div>

    <div class="chart-card">
      <div class="chart-header"><h3 class="chart-title">Glacier Mass Balance</h3></div>
      <div class="chart-content">
        ${resize((width) => {
           const glacierTempData = glaciersData.map(d => ({
            year: d.year, glacier: d.value,
            temp: globalTempData.find(t => t.year === d.year)?.value
          })).filter(d => d.temp !== undefined);
          return Plot.plot({
            width, height: 300, marginLeft: 50, marginBottom: 40,
            x: { label: "Temp Anomaly (°C)", grid: true },
            y: { label: "Glacier Mass (m)", grid: true },
            color: { scheme: "YlOrRd" },
            marks: [
              Plot.dot(glacierTempData, {x: "temp", y: "glacier", fill: "year", r: 4, tip: true }),
              Plot.linearRegressionY(glacierTempData, {x: "temp", y: "glacier", stroke: "black", strokeDasharray: "4,4"})
            ]
          });
        })}
      </div>
    </div>
  </div>

  <div class="dashboard-bottom" style="grid-template-columns: 1fr 1fr;">
    <div class="chart-card">
      <div class="chart-header"><h3 class="chart-title">Annual CO₂ Increase Rate</h3></div>
      <div class="chart-content">
        ${resize((width) => {
          const yearlyAverage = d3.rollups(co2Data, v => d3.mean(v, d => d.average), d => d.yearInt)
            .map(([year, avg]) => ({ year, average: avg })).sort((a, b) => a.year - b.year);
          const yearlyRates = yearlyAverage.slice(1).map((d, i) => ({
            year: d.year, rate: d.average - yearlyAverage[i].average
          }));
          return Plot.plot({
            width, height: 260, marginLeft: 50, marginBottom: 40,
            y: { label: "PPM/Year", grid: true, nice: true, clamp: true },
            marks: [
              Plot.ruleY([0]),
              Plot.areaY(yearlyRates, {x: "year", y: "rate", fill: "#dc2626", fillOpacity: 0.1, curve: "catmull-rom"}),
              Plot.line(yearlyRates, {x: "year", y: "rate", stroke: "#dc2626", strokeWidth: 2, curve: "catmull-rom", clip: true}),
              Plot.dot(yearlyRates, {x: "year", y: "rate", r: 2, fill: "transparent", tip: true})
            ]
          });
        })}
      </div>
    </div>

    <div class="chart-card">
      <div class="chart-header"><h3 class="chart-title">Critical CO₂ Thresholds</h3></div>
      <div class="chart-content">
        ${resize((width) => {
          // Horizontal Gauge / Thermometer Style
          const height = 260;
          const container = d3.create("div").style("position", "relative");
          const svg = container.append("svg").attr("width", width).attr("height", height).attr("viewBox", [0, 0, width, height]);

          const marginLeft=20, marginRight=20, marginTop=40, marginBottom=20;
          const chartWidth = width - marginLeft - marginRight;
          const barHeight = 60;
          const barY = 100;

          const xScale = d3.scaleLinear().domain([250, 500]).range([marginLeft, width - marginRight]);

          // Zones
          const zones = [
             {min: 250, max: 280, color: "#22c55e", label: "Pre-Industrial"},
             {min: 280, max: 350, color: "#f59e0b", label: "Safe"},
             {min: 350, max: 450, color: "#f97316", label: "Risk"},
             {min: 450, max: 500, color: "#dc2626", label: "Crisis"}
          ];

          // Draw zones
          zones.forEach(z => {
             svg.append("rect")
                .attr("x", xScale(z.min))
                .attr("y", barY)
                .attr("width", xScale(z.max) - xScale(z.min))
                .attr("height", barHeight)
                .attr("fill", z.color)
                .attr("fill-opacity", 0.3)
                .attr("stroke", "white");

             // Labels
             if (width > 400 || (z.label !== "Risk" && z.label !== "Safe")) {
                svg.append("text")
                   .attr("x", xScale(z.min) + 5)
                   .attr("y", barY + barHeight + 15)
                   .text(z.label)
                   .attr("font-size", "10px")
                   .attr("fill", "#555")
                   .attr("font-weight", "600");
             }
          });

          // Axis
          svg.append("g").attr("transform", `translate(0, ${barY})`)
             .call(d3.axisTop(xScale).tickValues([280, 350, 450]).tickFormat(d => d));

          // Current Value Marker
          const currentVal = latestCO2.average;
          const cx = xScale(currentVal);

          svg.append("line")
             .attr("x1", cx).attr("x2", cx)
             .attr("y1", barY - 10).attr("y2", barY + barHeight + 10)
             .attr("stroke", "#dc2626").attr("stroke-width", 3);

          // Current Value Label (Tooltip style box)
          const labelX = Math.min(Math.max(cx, marginLeft + 50), width - marginRight - 50);
          svg.append("rect")
             .attr("x", labelX - 45).attr("y", barY - 45)
             .attr("width", 90).attr("height", 30).attr("rx", 4)
             .attr("fill", "#dc2626");

          svg.append("text")
             .attr("x", labelX).attr("y", barY - 26)
             .attr("text-anchor", "middle")
             .attr("fill", "white")
             .attr("font-weight", "bold")
             .attr("font-size", "12px")
             .text(`${currentVal.toFixed(1)} PPM`);

          // Triangle pointer
          const path = d3.path();
          path.moveTo(cx, barY - 10);
          path.lineTo(cx - 6, barY - 16);
          path.lineTo(cx + 6, barY - 16);
          path.closePath();
          svg.append("path").attr("d", path).attr("fill", "#dc2626");

          return container.node();
        })}
      </div>
    </div>
  </div>

  <div class="dashboard-footer">
    <span>Built with Observable Framework</span>
    <span>Sources: NASA, CSIRO, WGMS, Scripps</span>
  </div>
</div>`)
```
