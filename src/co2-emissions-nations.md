---
title: Global CO2 Emissions by Nation
toc: false
---

<style>
.hero {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  padding: 0.5rem 0.75rem;
  background: var(--theme-background-alt);
  color: var(--theme-foreground);
  border-radius: 4px;
  margin-bottom: 0.75rem;
  border: 2px solid var(--theme-foreground-faint);
  overflow-anchor: none !important;
}

.hero h1 {
  margin: 0;
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--theme-foreground-alt);
  max-width: 100%;
  line-height: 1.2;
}

.hero p {
  margin: 0.15rem 0 0;
  font-size: 0.7rem;
  color: var(--theme-foreground-muted);
  max-width: 100%;
  line-height: 1.3;
}

* {
  overflow-anchor: none !important;
}

body {
  overflow-anchor: none !important;
}

.card, .card > *, .card svg, .card figure {
  overflow-anchor: none !important;
  contain: layout style paint;
}

.card, .grid {
  max-width: 100% !important;
}

.chart-container {
  overflow-anchor: none !important;
  contain: layout style paint;
  min-height: 340px;
}

.chart-grid {
  overflow-anchor: none !important;
}

svg, svg * {
  overflow-anchor: none !important;
}

.dashboard-layout {
  display: grid;
  grid-template-columns: 210px 1fr;
  gap: 1rem;
  margin: 0 0 4rem 0;
  overflow-anchor: none !important;
  min-height: 0;
}

.sidebar {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding-right: 0.5rem;
  border-right: 1px solid var(--theme-foreground-faint);
}

.stat-card {
  background: var(--theme-background-alt);
  border: 2px solid var(--theme-foreground-faint);
  border-radius: 4px;
  padding: 0.5rem;
  text-align: left;
  overflow-anchor: none !important;
  min-height: 60px;
}

.stat-value {
  font-size: 1.3rem;
  font-weight: 900;
  color: var(--theme-foreground-alt);
  margin: 0.1rem 0;
  line-height: 1.1;
}

.stat-label {
  font-size: 0.6rem;
  color: var(--theme-foreground-muted);
  text-transform: uppercase;
  letter-spacing: 0.03em;
  font-weight: 600;
  line-height: 1.2;
}

.stat-change {
  font-size: 0.6rem;
  font-weight: 500;
  margin-top: 0.1rem;
  color: var(--theme-foreground-muted);
  line-height: 1.2;
}

.main-content {
  display: flex;
  flex-direction: column;
  gap: 0;
  min-height: 400px;
  background: white;
  border: 1px solid var(--theme-foreground-faint);
  border-radius: 6px;
  overflow: visible;
}

.chart-grid {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-template-rows: auto auto;
  gap: 0;
  overflow-anchor: none !important;
  contain: layout;
}

.chart-grid .chart-container {
  border: none;
  border-radius: 0;
  position: relative;
}

.chart-grid .chart-container:nth-child(1) {
  grid-column: 1 / 3;
  border-bottom: 1px solid #e5e7eb;
}

.chart-grid .chart-container:nth-child(2)::after {
  content: "";
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  width: 1px;
  background: #e5e7eb;
}

@media (max-width: 1024px) {
  .dashboard-layout {
    grid-template-columns: 1fr;
  }

  .sidebar {
    flex-direction: row;
    flex-wrap: wrap;
    border-right: none;
    border-bottom: 1px solid var(--theme-foreground-faint);
    padding-right: 0;
    padding-bottom: 0.75rem;
  }

  .controls-section {
    width: 100%;
  }

  .stat-card {
    flex: 1;
    min-width: 180px;
  }
}

@media (max-width: 768px) {
  .chart-grid {
    grid-template-columns: 1fr;
    grid-template-rows: auto;
  }

  .chart-grid .chart-container:nth-child(1) {
    grid-column: auto;
  }

  .chart-grid .chart-container:nth-child(2)::after {
    width: 100%;
    height: 1px;
    right: auto;
    left: 0;
    top: auto;
    bottom: 0;
  }

  .chart-container {
    min-height: 280px;
  }

  .sidebar {
    flex-direction: column;
  }

  .stat-card {
    min-width: auto;
  }

  .dashboard-layout {
    max-height: none;
  }

  .main-content {
    max-height: none;
  }
}

.chart-container {
  background: white;
  border: none;
  border-radius: 0;
  padding: 0.6rem;
}

.chart-container h3 {
  margin: 0 0 0.4rem 0;
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--theme-foreground);
  overflow-anchor: none !important;
}

.chart-container > * {
  overflow-anchor: none !important;
}

.controls-section {
  background: var(--theme-background-alt);
  border: 2px solid var(--theme-foreground-faint);
  border-radius: 4px;
  padding: 0.5rem;
}

.controls-section h3 {
  margin: 0 0 0.35rem 0;
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--theme-foreground);
  text-transform: uppercase;
  letter-spacing: 0.03em;
}

.insights-section {
  background: var(--theme-background-alt);
  border: 2px solid var(--theme-foreground-faint);
  border-radius: 4px;
  padding: 0.6rem;
  font-size: 0.7rem;
  line-height: 1.35;
}

.insights-section h3 {
  margin: 0 0 0.4rem 0;
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--theme-foreground);
  text-transform: uppercase;
  letter-spacing: 0.03em;
}

.insights-section ul {
  margin: 0;
  padding-left: 1rem;
  list-style-type: none;
}

.insights-section li {
  margin-bottom: 0.35rem;
  position: relative;
  padding-left: 0.4rem;
  font-size: 0.68rem;
  line-height: 1.4;
}

.insights-section li::before {
  content: "•";
  position: absolute;
  left: -0.4rem;
  color: var(--theme-foreground-focus);
  font-weight: bold;
  font-size: 0.7rem;
}

.insights-section li:last-child {
  margin-bottom: 0;
}

.insights-section .data-sources {
  margin-top: 0.55rem;
  padding-top: 0.55rem;
  border-top: 1px solid var(--theme-foreground-faint);
  font-size: 0.62rem;
  color: var(--theme-foreground-muted);
  line-height: 1.35;
}

form, form * {
  overflow-anchor: none !important;
}
</style>

<div class="hero">
  <h1>Global CO₂ Emissions by Nation</h1>
  <p>Fossil Fuel & Cement Emissions by Nation (1751-2020)</p>
</div>

```js
// Load emissions data
const emissionsRaw = FileAttachment("data/fossil-fuel-co2-emissions-by-nation.csv").csv({typed: true});
const emissionsData = (await emissionsRaw).map(d => ({
  year: +d.Year,
  country: d.Country,
  total: +d.Total || 0,
  solidFuel: +d["Solid Fuel"] || 0,
  liquidFuel: +d["Liquid Fuel"] || 0,
  gasFuel: +d["Gas Fuel"] || 0,
  cement: +d.Cement || 0,
  gasFlaring: +d["Gas Flaring"] || 0,
  perCapita: +d["Per Capita"] || 0
})).filter(d => d.total > 0);

// Get latest year data
const latestYear = d3.max(emissionsData, d => d.year);
const latestYearData = emissionsData.filter(d => d.year === latestYear);

// Calculate global total
const globalTotal = d3.sum(latestYearData, d => d.total);

// Get top emitters
const topEmitters = latestYearData
  .sort((a, b) => b.total - a.total)
  .slice(0, 20);

// Get top per capita
const topPerCapita = latestYearData
  .filter(d => d.perCapita > 0)
  .sort((a, b) => b.perCapita - a.perCapita)
  .slice(0, 10);
```

```js
function Scrubber(values, {format = value => value, initial = 0, direction = 1, delay = null, autoplay = true, loop = true, loopDelay = null, alternate = false} = {}) {
  values = Array.from(values);
  const form = html`<form style="font: 9px var(--sans-serif); display: flex; flex-direction: column; gap: 0.35rem;"><button name=b type=button style="width: 100%; padding: 0.25rem; font-size: 9px;"></button><label style="display: flex; flex-direction: column; gap: 0.2rem;"><input name=i type=range min=0 max=${values.length - 1} value=${initial} step=1 style="width: 100%;"><output name=o style="font-size: 10px; font-weight: 600; text-align: center;"></output></label></form>`;
  let frame = null, timer = null, interval = null;
  const start = () => { form.b.textContent = "Pause"; if (delay === null) frame = requestAnimationFrame(tick); else interval = setInterval(tick, delay); };
  const stop = () => { form.b.textContent = "Play"; if (frame !== null) cancelAnimationFrame(frame), frame = null; if (timer !== null) clearTimeout(timer), timer = null; if (interval !== null) clearInterval(interval), interval = null; };
  const running = () => frame !== null || timer !== null || interval !== null;
  const step = () => { form.i.valueAsNumber = (form.i.valueAsNumber + direction + values.length) % values.length; form.i.dispatchEvent(new CustomEvent("input", {bubbles: true})); };
  const tick = () => { if (form.i.valueAsNumber === (direction > 0 ? values.length - 1 : direction < 0 ? 0 : NaN)) { if (!loop) return stop(); if (alternate) direction = -direction; if (loopDelay !== null) { if (frame !== null) cancelAnimationFrame(frame), frame = null; if (interval !== null) clearInterval(interval), interval = null; timer = setTimeout(() => (step(), start()), loopDelay); return; } } if (delay === null) frame = requestAnimationFrame(tick); step(); };
  form.i.oninput = event => { if (event && event.isTrusted && running()) stop(); form.value = values[form.i.valueAsNumber]; form.o.value = format(form.value, form.i.valueAsNumber, values); };
  form.b.onclick = () => { if (running()) return stop(); direction = alternate && form.i.valueAsNumber === values.length - 1 ? -1 : 1; form.i.valueAsNumber = (form.i.valueAsNumber + direction) % values.length; form.i.dispatchEvent(new CustomEvent("input", {bubbles: true})); start(); };
  form.i.oninput();
  if (autoplay) start(); else stop();
  Inputs.disposal(form).then(stop);
  return form;
}
```

<div class="dashboard-layout">
  <div class="sidebar">
    <div class="controls-section">
      <h3>Timeline</h3>

```js
const dashboardYears = d3.range(1751, latestYear + 1);
const selectedDashboardYear = view(Scrubber(dashboardYears, {
  delay: 100,
  loop: false,
  initial: dashboardYears.length - 1,
  autoplay: false,
  format: d => d
}));
```

  </div>

```js
const yearData = emissionsData
  .filter(d => d.year === selectedDashboardYear && d.total > 0)
  .sort((a, b) => b.total - a.total)
  .slice(0, 20);

const yearGlobalTotal = d3.sum(yearData, d => d.total);
const yearTopEmitter = yearData[0];
const yearCountriesCount = yearData.length;
const yearTop5Total = d3.sum(yearData.slice(0, 5), d => d.total);
const yearTop5Percentage = ((yearTop5Total / yearGlobalTotal) * 100).toFixed(0);
const yearAvgPerCountry = (yearGlobalTotal / yearCountriesCount).toFixed(1);
```
  <div class="stat-card">
    <div class="stat-label">Global Total</div>
    <div class="stat-value">${(yearGlobalTotal / 1000).toFixed(1)}B</div>
    <div class="stat-change">Tons (${selectedDashboardYear})</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Top Emitter</div>
    <div class="stat-value">${yearTopEmitter?.country || 'N/A'}</div>
    <div class="stat-change">${yearTopEmitter ? (yearTopEmitter.total / 1000).toFixed(1) + 'B tons' : 'N/A'}</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Countries</div>
    <div class="stat-value">${yearCountriesCount}</div>
    <div class="stat-change">Reporting</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Top 5 Share</div>
    <div class="stat-value">${yearTop5Percentage}%</div>
    <div class="stat-change">Of Global Total</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Average/Country</div>
    <div class="stat-value">${yearAvgPerCountry}</div>
    <div class="stat-change">Million Tons</div>
  </div>
  <div class="insights-section">
    <h3>Key Insights</h3>
    <ul>
      <li><strong>Dataset:</strong> 270 years of global CO₂ emissions data (1751-2020)</li>
      <li><strong>Coverage:</strong> Tracks emissions from fossil fuels, cement production, and gas flaring</li>
      <li><strong>Top Sources:</strong> Solid fuel (coal), liquid fuel (oil), and natural gas dominate</li>
      <li><strong>Historical Shift:</strong> UK led in 1800s, USA in 1900s, China in 2000s</li>
      <li><strong>Interactive:</strong> Use timeline to explore how emissions evolved globally</li>
      <li><strong>Concentration:</strong> Top 5 countries consistently account for majority of emissions</li>
    </ul>
    <div class="data-sources">
      <strong>Data:</strong> <a href="https://datahub.io/core/co2-fossil-by-nation" target="_blank" rel="noopener noreferrer">CDIAC, 1751-2020</a>
    </div>
  </div>
  </div>

  <div class="main-content">

```js
const topCountries = yearData.slice(0, 12);

function shortenCountryName(name) {
  const abbreviations = {
    "UNITED STATES OF AMERICA": "USA",
    "UNITED KINGDOM": "UK",
    "RUSSIAN FEDERATION": "Russia",
    "SOVIET UNION": "USSR",
    "CHINA (MAINLAND)": "China",
    "FRANCE (INCLUDING MONACO)": "France",
    "GERMANY": "Germany",
    "UNITED ARAB EMIRATES": "UAE",
    "KOREA, REPUBLIC OF": "South Korea",
    "KOREA, DEMOCRATIC PEOPLE'S REPUBLIC OF": "North Korea",
    "IRAN (ISLAMIC REPUBLIC OF)": "Iran",
    "SAUDI ARABIA": "Saudi Arabia",
    "VENEZUELA (BOLIVARIAN REPUBLIC OF)": "Venezuela",
    "UNITED REPUBLIC OF TANZANIA": "Tanzania",
    "BOLIVIA (PLURINATIONAL STATE OF)": "Bolivia",
    "VIET NAM": "Vietnam"
  };
  return abbreviations[name] || name;
}

function emissionsBreakdown({width} = {}) {
  const isMobile = width < 640;
  const breakdownData = topCountries.flatMap(d => [
    { country: shortenCountryName(d.country), fullName: d.country, type: "Solid Fuel", value: d.solidFuel },
    { country: shortenCountryName(d.country), fullName: d.country, type: "Liquid Fuel", value: d.liquidFuel },
    { country: shortenCountryName(d.country), fullName: d.country, type: "Gas Fuel", value: d.gasFuel },
    { country: shortenCountryName(d.country), fullName: d.country, type: "Cement", value: d.cement },
    { country: shortenCountryName(d.country), fullName: d.country, type: "Gas Flaring", value: d.gasFlaring }
  ]);

  return Plot.plot({
    width,
    height: isMobile ? 280 : 320,
    marginLeft: isMobile ? 80 : 95,
    marginBottom: isMobile ? 50 : 40,
    marginTop: 10,
    marginRight: isMobile ? 20 : 30,
    style: {
      fontSize: isMobile ? "10px" : "11px"
    },
    x: {
      label: null,
      grid: true
    },
    y: {
      label: null
    },
    color: {
      legend: true,
      domain: ["Solid Fuel", "Liquid Fuel", "Gas Fuel", "Cement", "Gas Flaring"],
      range: ["#64748b", "#f59e0b", "#ef4444", "#8b5cf6", "#ec4899"],
      label: null
    },
    marks: [
      Plot.barX(breakdownData, {
        x: "value",
        y: "country",
        fill: "type",
        tip: true,
        title: d => `${d.fullName} - ${d.type}\n${d.value.toLocaleString()} million metric tons`,
        sort: { y: "x", reverse: true }
      }),
      Plot.ruleX([0])
    ]
  });
}
```

```js
function emissionsTreemap({width} = {}) {
  const isMobile = width < 640;
  const height = isMobile ? 280 : 320;

  const cumulativeToYear = d3.rollups(
    emissionsData.filter(d => d.year <= selectedDashboardYear),
    v => d3.sum(v, d => d.total),
    d => d.country
  )
    .map(([country, cumulative]) => ({
      country,
      cumulative
    }))
    .sort((a, b) => b.cumulative - a.cumulative)
    .slice(0, 30);

  const hierarchyData = {
    name: "Global",
    children: cumulativeToYear.map(d => ({
      name: d.country,
      value: d.cumulative
    }))
  };

  const root = d3.hierarchy(hierarchyData)
    .sum(d => d.value)
    .sort((a, b) => b.value - a.value);

  d3.treemap()
    .size([width, height])
    .paddingInner(3)
    .paddingOuter(6)
    .round(true)(root);

  const colorScale = d3.scaleSequential(d3.interpolateRdYlGn)
    .domain([d3.max(cumulativeToYear, d => d.cumulative), 0]);

  const container = d3.create("div")
    .style("position", "relative");

  const svg = container.append("svg")
    .attr("width", width)
    .attr("height", height)
    .attr("viewBox", [0, 0, width, height])
    .style("background", "#ffffff")
    .style("font", "13px sans-serif")
    .style("display", "block");

  const tooltip = container.append("div")
    .style("position", "absolute")
    .style("visibility", "hidden")
    .style("background", "white")
    .style("border", "1px solid #ccc")
    .style("border-radius", "4px")
    .style("padding", "8px")
    .style("font-size", "12px")
    .style("font-family", "system-ui, sans-serif")
    .style("pointer-events", "none")
    .style("box-shadow", "0 2px 4px rgba(0,0,0,0.1)")
    .style("z-index", "1000")
    .style("line-height", "1.5");

  const leaf = svg.selectAll("g")
    .data(root.leaves())
    .join("g")
      .attr("transform", d => `translate(${d.x0},${d.y0})`);

  leaf.append("rect")
    .attr("fill", d => colorScale(d.data.value))
    .attr("fill-opacity", 0.85)
    .attr("width", d => d.x1 - d.x0)
    .attr("height", d => d.y1 - d.y0)
    .attr("stroke", "#e5e7eb")
    .attr("stroke-width", 3)
    .attr("rx", 4)
    .style("cursor", "pointer")
    .on("mouseenter", function(event, d) {
      d3.select(this)
        .attr("fill-opacity", 1)
        .attr("stroke", "#fbbf24")
        .attr("stroke-width", 4);

      tooltip
        .html(`<strong>${d.data.name}</strong><br/>Cumulative (1751-${selectedDashboardYear}):<br/>${d.data.value.toLocaleString()} MT`)
        .style("visibility", "visible");
    })
    .on("mousemove", function(event) {
      const [mx, my] = d3.pointer(event, container.node());
      const tooltipNode = tooltip.node();
      const tooltipWidth = tooltipNode.offsetWidth;
      const tooltipHeight = tooltipNode.offsetHeight;

      let left = mx + 15;
      let top = my - tooltipHeight / 2;

      if (left + tooltipWidth > width) {
        left = mx - tooltipWidth - 15;
      }
      if (top < 0) top = 0;
      if (top + tooltipHeight > height) top = height - tooltipHeight;

      tooltip
        .style("left", `${left}px`)
        .style("top", `${top}px`);
    })
    .on("mouseleave", function() {
      d3.select(this)
        .attr("fill-opacity", 0.85)
        .attr("stroke", "#e5e7eb")
        .attr("stroke-width", 3);

      tooltip.style("visibility", "hidden");
    });

  leaf.append("clipPath")
    .attr("id", (d, i) => `clip-${i}`)
    .append("rect")
    .attr("width", d => d.x1 - d.x0)
    .attr("height", d => d.y1 - d.y0);

  // Country name
  leaf.append("text")
    .attr("clip-path", (d, i) => `url(#clip-${i})`)
    .attr("x", 5)
    .attr("y", isMobile ? 14 : 16)
    .attr("fill", "white")
    .attr("font-weight", "bold")
    .attr("font-size", d => {
      const width = d.x1 - d.x0;
      const height = d.y1 - d.y0;
      if (width < 30 || height < 20) return 0;
      return Math.min(width / 8, height / 4, isMobile ? 12 : 14);
    })
    .style("text-shadow", "0 1px 3px rgba(0,0,0,0.4), 0 0 2px rgba(0,0,0,0.3)")
    .text(d => {
      const width = d.x1 - d.x0;
      if (width < 30) return "";
      const name = d.data.name;
      const maxChars = Math.floor(width / 6);
      if (name.length > maxChars) {
        return name.substring(0, maxChars - 1) + ".";
      }
      return name;
    })
    .style("pointer-events", "none");

  leaf.append("text")
    .attr("clip-path", (d, i) => `url(#clip-${i})`)
    .attr("x", 5)
    .attr("y", d => {
      const height = d.y1 - d.y0;
      const width = d.x1 - d.x0;
      if (width < 30 || height < 20) return 0;
      return height < 40 ? 26 : (isMobile ? 28 : 32);
    })
    .attr("fill", "white")
    .attr("font-size", d => {
      const width = d.x1 - d.x0;
      const height = d.y1 - d.y0;
      if (width < 30 || height < 20) return 0;
      return Math.min(width / 10, height / 5, isMobile ? 11 : 12);
    })
    .attr("font-weight", "600")
    .style("text-shadow", "0 1px 3px rgba(0,0,0,0.4), 0 0 2px rgba(0,0,0,0.3)")
    .text(d => {
      const width = d.x1 - d.x0;
      const height = d.y1 - d.y0;
      if (width < 30 || height < 20) return "";
      const val = d.data.value;
      if (val >= 1000) return `${(val / 1000).toFixed(1)}K`;
      return `${val.toFixed(0)}`;
    })
    .style("pointer-events", "none");

  return container.node();
}
```

```js
const top5Countries = topEmitters.slice(0, 5).map(d => d.country);

const historicalData = emissionsData.filter(d =>
  top5Countries.includes(d.country) &&
  d.year >= 1850
);

function historicalTrends({width} = {}) {
  const isMobile = width < 640;
  return Plot.plot({
    width,
    height: isMobile ? 280 : 320,
    marginLeft: isMobile ? 45 : 55,
    marginRight: isMobile ? 10 : 15,
    marginTop: 10,
    marginBottom: isMobile ? 50 : 45,
    style: {
      fontSize: isMobile ? "10px" : "11px"
    },
    x: {
      label: null,
      grid: true,
      tickFormat: "d",
      ticks: isMobile ? 5 : 8
    },
    y: {
      label: null,
      grid: true,
      type: "log",
      ticks: isMobile ? [1, 100, 10000] : [1, 10, 100, 1000, 10000]
    },
    color: {
      legend: true,
      domain: top5Countries,
      range: ["#ef4444", "#f59e0b", "#22c55e", "#3b82f6", "#8b5cf6"],
      label: null
    },
    marks: [
      Plot.line(historicalData, {
        x: "year",
        y: "total",
        stroke: "country",
        strokeWidth: 2,
        curve: "catmull-rom",
        tip: true,
        title: d => `${d.country} (${d.year})\n${d.total.toLocaleString()} MT`
      }),
      Plot.dot(
        historicalData.filter(d => d.year === latestYear),
        {
          x: "year",
          y: "total",
          fill: "country",
          r: 4,
          stroke: "white",
          strokeWidth: 1.5
        }
      )
    ]
  });
}
```

<div class="chart-grid">
  <div class="chart-container">
    <h3>Emissions Breakdown by Source</h3>
    ${resize((width) => emissionsBreakdown({width}))}
  </div>

  <div class="chart-container">
    <h3>Historical Trends: Top 5 Emitters</h3>
    ${resize((width) => historicalTrends({width}))}
  </div>

  <div class="chart-container">
    <h3>Cumulative Emissions (1751-${selectedDashboardYear})</h3>
    ${resize((width) => emissionsTreemap({width}))}
  </div>
</div>

  </div>
</div>
