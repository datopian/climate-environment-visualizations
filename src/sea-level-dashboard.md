---
title: Global Sea Level Rise Dashboard
toc: false
---

<style>
.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 3rem 2rem;
  background: var(--theme-background-alt);
  color: var(--theme-foreground);
  border-radius: 16px;
  margin-bottom: 2rem;
  border: 2px solid var(--theme-foreground-faint);
}

.hero h1 {
  margin: 0;
  font-size: 3rem;
  font-weight: 900;
  color: var(--theme-foreground-alt);
  max-width: 100%;
}

.hero p {
  margin: 1rem 0 0;
  font-size: 1.2rem;
  color: var(--theme-foreground-muted);
  max-width: 100%;
}

svg rect, svg path, svg circle, svg text {
  transition: opacity 0.5s ease-in-out, fill 0.5s ease-in-out;
}

* {
  overflow-anchor: none !important;
}

.card, .card > *, .card svg, .card figure {
  overflow-anchor: none !important;
  contain: layout style paint;
}

.card, .grid {
  max-width: 100% !important;
}

.card {
  min-height: 450px;
}
</style>

<div class="hero">
  <h1>Global Mean Sea Level Rise Data Story</h1>
  <p>This data contains cumulative changes in sea level for the world’s oceans since 1880, based on a combination of long-term tide gauge measurements and recent satellite measurements.</p>
</div>

```js
// Load data
const epaRaw = FileAttachment("data/epa-sea-level.csv").csv({typed: true});
const csiroRaw = FileAttachment("data/CSIRO_Recons_gmsl_yr_2019.csv").csv({typed: true});
const satRaw = FileAttachment("data/CSIRO_Alt_yearly.csv").csv({typed: true});
const glaciersRaw = FileAttachment("data/glaciers.csv").csv({typed: true});
const globalTempRaw = FileAttachment("data/global-temp-annual.csv").csv({typed: true});
```

```js
const epaData = (await epaRaw).map(d => ({
  year: +d.Year,
  value: +d["CSIRO Adjusted Sea Level"] * 25.4, // Convert inches to mm
  lower: +d["Lower Error Bound"] * 25.4,
  upper: +d["Upper Error Bound"] * 25.4,
  source: "EPA"
})).filter(d => d.year >= 1880 && d.year <= 2023 && !isNaN(d.value) && d.value !== 0);

const csiroData = (await csiroRaw).map(d => ({
  year: +d.Time,
  value: +d.GMSL,
  source: "CSIRO"
})).filter(d => d.year >= 1880);

const satData = (await satRaw).map(d => ({
  year: +d.Time,
  value: +d["GMSL (yearly)"],
  source: "Satellite"
})).filter(d => d.year >= 1993);

const glaciersData = (await glaciersRaw).map(d => ({
  year: +d.Year,
  value: +d["Mean cumulative mass balance"],
  observations: +d["Number of observations"]
})).filter(d => !isNaN(d.value) && d.year >= 1956);

const globalTempData = (await globalTempRaw)
  .filter(d => d.Source === "GISTEMP")
  .map(d => ({
    year: +d.Year,
    value: +d.Mean
  }))
  .filter(d => !isNaN(d.value) && d.year >= 1880);

const allData = [...epaData, ...csiroData, ...satData];
```

```js
// Get the latest year with actual data
const latestYear = Math.max(...csiroData.map(d => d.year));
```

```js
// This function was implemented manually because ObservableHQ framework doesnt have the necessary lib for the framework, the lib only works in their notebook*...
function Scrubber(values, {format = value => value, initial = 0, direction = 1, delay = null, autoplay = true, loop = true, loopDelay = null, alternate = false} = {}) {
  values = Array.from(values);
  const form = html`<form style="font: 12px var(--sans-serif); display: flex; height: 33px; align-items: center;"><button name=b type=button style="margin-right: 0.4em; width: 5em;"></button><label style="display: flex; align-items: center;"><input name=i type=range min=0 max=${values.length - 1} value=${initial} step=1 style="width: 180px;"><output name=o style="margin-left: 0.4em;"></output></label></form>`;
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

```js
// Current stats
const currentEPA = epaData.find(d => d.year === selectedYear);
const currentCSIRO = csiroData.find(d => d.year === selectedYear);
const currentSat = satData.find(d => d.year === selectedYear);
```

---
## About This Data

**Global Average Absolute Sea Level Change, 1880-2014** from the US Environmental Protection Agency using data from CSIRO, 2015; NOAA, 2015.

This data contains cumulative changes in sea level for the world's oceans since 1880, based on a combination of long-term tide gauge measurements and recent satellite measurements.

It shows **average absolute sea level change**, which refers to the height of the ocean surface, regardless of whether nearby land is rising or falling.

Satellite data are based solely on measured sea level, while the long-term tide gauge data include a small correction factor because the size and shape of the oceans are changing slowly over time. *(On average, the ocean floor has been gradually sinking since the last Ice Age peak, 20,000 years ago.)*

---

## Data Sources

- **EPA**
  - Name: EPA (United States Environmental Protection Agency)
  - Web: http://www3.epa.gov/climatechange/images/indicator_downloads/sea-level_fig-1.csv

- **CSIRO**
  - Name: CSIRO (Commonwealth Scientific and Industrial Research Organization)
  - Web: http://www.cmar.csiro.au/sealevel/GMSL_SG_2011_up.html

- **Satellite**
  - Name: Satellite csiro Altimetry Data
  - Web: https://www.cmar.csiro.au/sealevel/sl_meas_sat_alt.html
---

## Sea Level Change Over Time

This line chart displays sea level measurements from all three datasets over time. **EPA** data (1880-2013) is shown in purple with uncertainty bands. **CSIRO** data (1880-2019) is shown in green. **Satellite** data (1993-2020) is shown in red. All data is displayed in millimeters (EPA converted from inches).

```js
const selectedDataset = view(Inputs.select(
  ["All", "EPA", "CSIRO", "Satellite"],
  {label: "Source", value: "All"}
));
```

```js
// Filter data
const filteredData = allData.filter(d => {
  if (selectedDataset === "All") return d.year <= selectedYear;
  if (selectedDataset === "EPA") return d.source === "EPA" && d.year <= selectedYear;
  if (selectedDataset === "CSIRO") return d.source === "CSIRO" && d.year <= selectedYear;
  if (selectedDataset === "Satellite") return d.source === "Satellite" && d.year <= selectedYear;
  return false;
});
```

```js
function seaLevelChart(data, {width} = {}) {
  return Plot.plot({
    title: "Global Mean Sea Level Rise",
    width,
    height: 400,
    marginLeft: 60,
    x: {label: "Year", grid: true, domain: [1880, latestYear], tickFormat: d => String(d)},
    y: {label: "Sea Level Change (mm)", grid: true},
    color: {
      legend: true,
      domain: ["EPA", "CSIRO", "Satellite"],
      range: ["#4f46e5", "#059669", "#dc2626"]
    },
    marks: [
      // Uncertainty band for EPA
      Plot.areaY(
        data.filter(d => d.source === "EPA" && d.lower !== undefined),
        {x: "year", y1: "lower", y2: "upper", fill: "#e0e7ff", fillOpacity: 0.3}
      ),
      // Lines for each dataset
      Plot.line(data, {
        x: "year",
        y: "value",
        stroke: "source",
        strokeWidth: 2,
        curve: "catmull-rom"
      }),
      // Current year marker
      Plot.ruleX([selectedYear], {stroke: "#f59e0b", strokeWidth: 2}),
      // Interactive dots
      Plot.dot(data, Plot.pointerX({
        x: "year",
        y: "value",
        stroke: "source",
        r: 4,
        title: d => `${d.source} (${d.year}): ${d.value.toFixed(1)} mm`
      })),
      Plot.ruleY([0])
    ]
  });
}
```

<div class="card">
  ${resize((width) => seaLevelChart(filteredData, {width}))}
</div>

```js
const years = d3.range(1880, latestYear + 1);
const selectedYear = view(Scrubber(years, {
  delay: 150,
  loop: false,
  initial: years.length - 1,
  autoplay: false,
  format: d => String(d)
}));
```

---

## ❄️ Sea Level Rise & Glacier Mass Balance Correlation

This multi-axis visualization shows the relationship between global sea level rise and glacier mass loss from 1956 to 2019. As glaciers lose mass (shown as increasingly negative values on the right axis), sea level rises (left axis). The inverse correlation reveals how glacier melt contributes significantly to sea level rise.

```js
const correlationYears = d3.range(1956, 2020);
const selectedCorrelationYear = view(Scrubber(correlationYears, {
  delay: 150,
  loop: false,
  initial: correlationYears.length - 1,
  autoplay: false,
  format: d => String(d)
}));
```

```js
const correlationSeaLevel = csiroData.filter(d => d.year >= 1956 && d.year <= 2019 && d.year <= selectedCorrelationYear);
const correlationGlaciers = glaciersData.filter(d => d.year >= 1956 && d.year <= 2019 && d.year <= selectedCorrelationYear);
```

```js
function glacierMultiAxisChart({width} = {}) {
  const isMobile = width < 640;
  const height = isMobile ? 350 : 400;
  const marginTop = 40;
  const marginRight = isMobile ? 50 : 100;
  const marginBottom = isMobile ? 100 : 80;
  const marginLeft = isMobile ? 50 : 80;

  const xScale = d3.scaleLinear()
    .domain([1956, 2019])
    .range([marginLeft, width - marginRight]);

  const yLeftScale = d3.scaleLinear()
    .domain([d3.min(correlationSeaLevel, d => d.value), d3.max(correlationSeaLevel, d => d.value)])
    .range([height - marginBottom, marginTop])
    .nice();

  const yRightScale = d3.scaleLinear()
    .domain([d3.min(glaciersData.filter(d => d.year >= 1956 && d.year <= 2019), d => d.value), d3.max(glaciersData.filter(d => d.year >= 1956 && d.year <= 2019), d => d.value)])
    .range([height - marginBottom, marginTop])
    .nice();

  const lineSeaLevel = d3.line()
    .x(d => xScale(d.year))
    .y(d => yLeftScale(d.value))
    .curve(d3.curveCatmullRom);

  const lineGlacier = d3.line()
    .x(d => xScale(d.year))
    .y(d => yRightScale(d.value))
    .curve(d3.curveCatmullRom);

  const svg = d3.create("svg")
    .attr("width", width)
    .attr("height", height)
    .attr("viewBox", [0, 0, width, height])
    .attr("style", "max-width: 100%; height: auto; background: white;");

  svg.append("g")
    .attr("class", "grid")
    .attr("transform", `translate(0,${height - marginBottom})`)
    .call(d3.axisBottom(xScale)
      .tickFormat("")
      .tickSize(-(height - marginTop - marginBottom)))
    .call(g => g.select(".domain").remove())
    .call(g => g.selectAll(".tick line")
      .attr("stroke", "#e5e7eb")
      .attr("stroke-opacity", 0.7));

  svg.append("g")
    .attr("class", "grid")
    .attr("transform", `translate(${marginLeft},0)`)
    .call(d3.axisLeft(yLeftScale)
      .tickFormat("")
      .tickSize(-(width - marginLeft - marginRight)))
    .call(g => g.select(".domain").remove())
    .call(g => g.selectAll(".tick line")
      .attr("stroke", "#e5e7eb")
      .attr("stroke-opacity", 0.7));

  svg.append("g")
    .attr("transform", `translate(0,${height - marginBottom})`)
    .call(d3.axisBottom(xScale).tickFormat(d3.format("d")).ticks(isMobile ? 5 : 10).tickSize(0))
    .call(g => g.append("text")
      .attr("x", width / 2)
      .attr("y", isMobile ? 75 : 65)
      .attr("fill", "currentColor")
      .attr("text-anchor", "middle")
      .attr("font-size", isMobile ? "11px" : "13px")
      .text("Year"));

  svg.append("g")
    .attr("transform", `translate(${marginLeft},0)`)
    .call(d3.axisLeft(yLeftScale).ticks(isMobile ? 5 : 8).tickSize(0).tickFormat(d => `${d}mm`))
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "12px"));

  svg.append("g")
    .attr("transform", `translate(${width - marginRight},0)`)
    .call(d3.axisRight(yRightScale).ticks(isMobile ? 5 : 8).tickSize(0).tickFormat(d => `${d}m`))
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "12px"));

  if (isMobile) {
    const legend = svg.append("g")
      .attr("transform", `translate(${width / 2 - 110}, ${height - 55})`);

    legend.append("line")
      .attr("x1", 0)
      .attr("x2", 20)
      .attr("y1", 0)
      .attr("y2", 0)
      .attr("stroke", "#059669")
      .attr("stroke-width", 2);

    legend.append("text")
      .attr("x", 23)
      .attr("y", 3)
      .attr("fill", "currentColor")
      .attr("font-size", "9px")
      .text("Sea Level (mm)");

    legend.append("line")
      .attr("x1", 0)
      .attr("x2", 20)
      .attr("y1", 15)
      .attr("y2", 15)
      .attr("stroke", "#3b82f6")
      .attr("stroke-width", 2);

    legend.append("text")
      .attr("x", 23)
      .attr("y", 18)
      .attr("fill", "currentColor")
      .attr("font-size", "9px")
      .text("Glacier Mass (m)");
  } else {
    const legend = svg.append("g")
      .attr("transform", `translate(${width / 2 - 200}, ${height - 35})`);

    legend.append("line")
      .attr("x1", 0)
      .attr("x2", 30)
      .attr("y1", 0)
      .attr("y2", 0)
      .attr("stroke", "#059669")
      .attr("stroke-width", 3);

    legend.append("text")
      .attr("x", 35)
      .attr("y", 4)
      .attr("fill", "currentColor")
      .attr("font-size", "12px")
      .text("Sea Level Rise (mm)");

    legend.append("line")
      .attr("x1", 200)
      .attr("x2", 230)
      .attr("y1", 0)
      .attr("y2", 0)
      .attr("stroke", "#3b82f6")
      .attr("stroke-width", 3);

    legend.append("text")
      .attr("x", 235)
      .attr("y", 4)
      .attr("fill", "currentColor")
      .attr("font-size", "12px")
      .text("Glacier Mass Balance (m)");
  }

  svg.append("path")
    .datum(correlationSeaLevel)
    .attr("fill", "none")
    .attr("stroke", "#059669")
    .attr("stroke-width", 3)
    .attr("d", lineSeaLevel);

  svg.append("path")
    .datum(correlationGlaciers)
    .attr("fill", "none")
    .attr("stroke", "#3b82f6")
    .attr("stroke-width", 3)
    .attr("d", lineGlacier);

  if (selectedCorrelationYear >= 1956) {
    svg.append("line")
      .attr("x1", xScale(selectedCorrelationYear))
      .attr("x2", xScale(selectedCorrelationYear))
      .attr("y1", marginTop)
      .attr("y2", height - marginBottom)
      .attr("stroke", "#f59e0b")
      .attr("stroke-width", 2)
      .attr("stroke-dasharray", "4,4");

    const seaLevelPoint = correlationSeaLevel.find(d => d.year === selectedCorrelationYear);
    if (seaLevelPoint) {
      svg.append("circle")
        .attr("cx", xScale(seaLevelPoint.year))
        .attr("cy", yLeftScale(seaLevelPoint.value))
        .attr("r", 6)
        .attr("fill", "#059669")
        .attr("stroke", "white")
        .attr("stroke-width", 2);
    }

    const glacierPoint = correlationGlaciers.find(d => d.year === selectedCorrelationYear);
    if (glacierPoint) {
      svg.append("circle")
        .attr("cx", xScale(glacierPoint.year))
        .attr("cy", yRightScale(glacierPoint.value))
        .attr("r", 6)
        .attr("fill", "#3b82f6")
        .attr("stroke", "white")
        .attr("stroke-width", 2);
    }
  }

  return svg.node();
}
```

<div class="card">
  ${resize((width) => glacierMultiAxisChart({width}))}
</div>

---

## 🌡️ Sea Level Rise & Global Temperature Correlation

This multi-axis visualization shows the relationship between global sea level rise and global temperature anomalies from 1880 to 2019. Temperature anomalies (right axis) represent deviations from the 1951-1980 baseline. Both trends rise together, demonstrating how thermal expansion of ocean water and accelerated ice melt from warming contribute to sea level rise (left axis).

```js
const tempCorrelationYears = d3.range(1880, 2020);
const selectedTempYear = view(Scrubber(tempCorrelationYears, {
  delay: 150,
  loop: false,
  initial: tempCorrelationYears.length - 1,
  autoplay: false,
  format: d => String(d)
}));
```

```js
const tempSeaLevel = csiroData.filter(d => d.year >= 1880 && d.year <= 2019 && d.year <= selectedTempYear);
const tempGlobalTemp = globalTempData.filter(d => d.year >= 1880 && d.year <= 2019 && d.year <= selectedTempYear);
```

```js
function temperatureMultiAxisChart({width} = {}) {
  const isMobile = width < 640;
  const height = isMobile ? 350 : 400;
  const marginTop = 40;
  const marginRight = isMobile ? 50 : 100;
  const marginBottom = isMobile ? 100 : 80;
  const marginLeft = isMobile ? 50 : 80;

  const xScale = d3.scaleLinear()
    .domain([1880, 2019])
    .range([marginLeft, width - marginRight]);

  const yLeftScale = d3.scaleLinear()
    .domain([d3.min(csiroData.filter(d => d.year >= 1880 && d.year <= 2019), d => d.value), d3.max(csiroData.filter(d => d.year >= 1880 && d.year <= 2019), d => d.value)])
    .range([height - marginBottom, marginTop])
    .nice();

  const yRightScale = d3.scaleLinear()
    .domain([d3.min(globalTempData.filter(d => d.year >= 1880 && d.year <= 2019), d => d.value), d3.max(globalTempData.filter(d => d.year >= 1880 && d.year <= 2019), d => d.value)])
    .range([height - marginBottom, marginTop])
    .nice();

  const lineSeaLevel = d3.line()
    .x(d => xScale(d.year))
    .y(d => yLeftScale(d.value))
    .curve(d3.curveCatmullRom);

  const lineTemp = d3.line()
    .x(d => xScale(d.year))
    .y(d => yRightScale(d.value))
    .curve(d3.curveCatmullRom);

  const svg = d3.create("svg")
    .attr("width", width)
    .attr("height", height)
    .attr("viewBox", [0, 0, width, height])
    .attr("style", "max-width: 100%; height: auto; background: white;");

  svg.append("g")
    .attr("class", "grid")
    .attr("transform", `translate(0,${height - marginBottom})`)
    .call(d3.axisBottom(xScale)
      .tickFormat("")
      .tickSize(-(height - marginTop - marginBottom)))
    .call(g => g.select(".domain").remove())
    .call(g => g.selectAll(".tick line")
      .attr("stroke", "#e5e7eb")
      .attr("stroke-opacity", 0.7));

  svg.append("g")
    .attr("class", "grid")
    .attr("transform", `translate(${marginLeft},0)`)
    .call(d3.axisLeft(yLeftScale)
      .tickFormat("")
      .tickSize(-(width - marginLeft - marginRight)))
    .call(g => g.select(".domain").remove())
    .call(g => g.selectAll(".tick line")
      .attr("stroke", "#e5e7eb")
      .attr("stroke-opacity", 0.7));

  svg.append("g")
    .attr("transform", `translate(0,${height - marginBottom})`)
    .call(d3.axisBottom(xScale).tickFormat(d3.format("d")).ticks(isMobile ? 5 : 10).tickSize(0))
    .call(g => g.append("text")
      .attr("x", width / 2)
      .attr("y", isMobile ? 75 : 65)
      .attr("fill", "currentColor")
      .attr("text-anchor", "middle")
      .attr("font-size", isMobile ? "11px" : "13px")
      .text("Year"));

  svg.append("g")
    .attr("transform", `translate(${marginLeft},0)`)
    .call(d3.axisLeft(yLeftScale).ticks(isMobile ? 5 : 8).tickSize(0).tickFormat(d => `${d}mm`))
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "12px"));

  svg.append("g")
    .attr("transform", `translate(${width - marginRight},0)`)
    .call(d3.axisRight(yRightScale).ticks(isMobile ? 5 : 8).tickSize(0).tickFormat(d => `${d}°C`))
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "12px"));

  if (isMobile) {
    const legend = svg.append("g")
      .attr("transform", `translate(${width / 2 - 110}, ${height - 55})`);

    legend.append("line")
      .attr("x1", 0)
      .attr("x2", 20)
      .attr("y1", 0)
      .attr("y2", 0)
      .attr("stroke", "#059669")
      .attr("stroke-width", 2);

    legend.append("text")
      .attr("x", 23)
      .attr("y", 3)
      .attr("fill", "currentColor")
      .attr("font-size", "9px")
      .text("Sea Level (mm)");

    legend.append("line")
      .attr("x1", 0)
      .attr("x2", 20)
      .attr("y1", 15)
      .attr("y2", 15)
      .attr("stroke", "#dc2626")
      .attr("stroke-width", 2);

    legend.append("text")
      .attr("x", 23)
      .attr("y", 18)
      .attr("fill", "currentColor")
      .attr("font-size", "9px")
      .text("Temperature (°C)");
  } else {
    const legend = svg.append("g")
      .attr("transform", `translate(${width / 2 - 220}, ${height - 35})`);

    legend.append("line")
      .attr("x1", 0)
      .attr("x2", 30)
      .attr("y1", 0)
      .attr("y2", 0)
      .attr("stroke", "#059669")
      .attr("stroke-width", 3);

    legend.append("text")
      .attr("x", 35)
      .attr("y", 4)
      .attr("fill", "currentColor")
      .attr("font-size", "12px")
      .text("Sea Level Rise (mm)");

    legend.append("line")
      .attr("x1", 200)
      .attr("x2", 230)
      .attr("y1", 0)
      .attr("y2", 0)
      .attr("stroke", "#dc2626")
      .attr("stroke-width", 3);

    legend.append("text")
      .attr("x", 235)
      .attr("y", 4)
      .attr("fill", "currentColor")
      .attr("font-size", "12px")
      .text("Temperature Anomaly (°C)");
  }

  svg.append("path")
    .datum(tempSeaLevel)
    .attr("fill", "none")
    .attr("stroke", "#059669")
    .attr("stroke-width", 3)
    .attr("d", lineSeaLevel);

  svg.append("path")
    .datum(tempGlobalTemp)
    .attr("fill", "none")
    .attr("stroke", "#dc2626")
    .attr("stroke-width", 3)
    .attr("d", lineTemp);

  if (selectedTempYear >= 1880) {
    svg.append("line")
      .attr("x1", xScale(selectedTempYear))
      .attr("x2", xScale(selectedTempYear))
      .attr("y1", marginTop)
      .attr("y2", height - marginBottom)
      .attr("stroke", "#f59e0b")
      .attr("stroke-width", 2)
      .attr("stroke-dasharray", "4,4");

    const seaLevelPoint = tempSeaLevel.find(d => d.year === selectedTempYear);
    if (seaLevelPoint) {
      svg.append("circle")
        .attr("cx", xScale(seaLevelPoint.year))
        .attr("cy", yLeftScale(seaLevelPoint.value))
        .attr("r", 6)
        .attr("fill", "#059669")
        .attr("stroke", "white")
        .attr("stroke-width", 2);
    }

    const tempPoint = tempGlobalTemp.find(d => d.year === selectedTempYear);
    if (tempPoint) {
      svg.append("circle")
        .attr("cx", xScale(tempPoint.year))
        .attr("cy", yRightScale(tempPoint.value))
        .attr("r", 6)
        .attr("fill", "#dc2626")
        .attr("stroke", "white")
        .attr("stroke-width", 2);
    }
  }

  return svg.node();
}
```

<div class="card">
  ${resize((width) => temperatureMultiAxisChart({width}))}
</div>

---

## 🌊 Horizon View - Compact Multi-Dataset Comparison

This horizon chart shows all three datasets in a compact, layered format. Each row represents one dataset (EPA, CSIRO, Satellite), with blue bands indicating the magnitude of sea level rise. Darker/higher bands represent higher sea level values. All data is displayed in millimeters (EPA converted from inches).

```js
function horizonChart({width} = {}) {
  const isMobile = width < 640;
  const bands = 4;
  const bandHeight = isMobile ? 40 : 50;
  const height = bandHeight * bands;
  const datasets = ["EPA", "CSIRO", "Satellite"];

  // Group data by source
  const epaOnly = epaData.filter(d => d.value > 0);
  const csiroOnly = csiroData.filter(d => d.value > 0);
  const satOnly = satData.filter(d => d.value > 0);

  const dataBySource = {
    "EPA": epaOnly,
    "CSIRO": csiroOnly,
    "Satellite": satOnly
  };

  // Calculate max value for scaling
  const maxValue = d3.max([...epaOnly, ...csiroOnly, ...satOnly], d => d.value);

  return Plot.plot({
    width,
    height: height * 3 + (isMobile ? 100 : 120),
    marginLeft: isMobile ? 50 : 100,
    marginRight: isMobile ? 10 : 20,
    marginTop: isMobile ? 20 : 30,
    marginBottom: isMobile ? 40 : 30,
    style: {
      fontSize: isMobile ? "10px" : "12px"
    },
    fy: {
      domain: datasets,
      label: null
    },
    x: {
      label: "Year",
      domain: [1880, latestYear],
      grid: true,
      ticks: isMobile ? 5 : 10,
      tickSize: 6,
      tickFormat: d => String(d)
    },
    y: {
      axis: null,
      domain: [0, bandHeight]
    },
    color: {
      scheme: "Blues",
      domain: [0, bands]
    },
    facet: {
      marginLeft: isMobile ? 50 : 100
    },
    marks: [
      // Draw overlapping bands for each dataset
      ...datasets.flatMap(source => {
        const data = dataBySource[source];
        return Array.from({length: bands}, (_, bandIndex) => {
          const threshold = (maxValue / bands) * bandIndex;

          return Plot.areaY(data, {
            fy: () => source,
            x: "year",
            y1: 0,
            y2: d => {
              const excess = d.value - threshold;
              return excess > 0 ? Math.min(bandHeight, (excess / (maxValue / bands)) * bandHeight) : 0;
            },
            fill: d3.interpolateBlues((bandIndex + 1) / bands),
            fillOpacity: 0.8,
            curve: "catmull-rom"
          });
        });
      }),
      // Source labels
      Plot.text(datasets, (d, i) => [{
        fy: d,
        x: 1880,
        y: bandHeight / 2,
        text: d,
        source: d
      }], {
        x: "x",
        y: "y",
        text: "text",
        fy: "fy",
        dx: isMobile ? -5 : -10,
        textAnchor: "end",
        fill: d => d === "EPA" ? "#4f46e5" : d === "CSIRO" ? "#059669" : "#dc2626",
        fontSize: isMobile ? 10 : 12,
        fontWeight: "bold"
      }),
      // Interactive tooltip
      Plot.tip(
        datasets.flatMap(source =>
          dataBySource[source].map(d => ({...d, source}))
        ),
        Plot.pointerX({
          fy: "source",
          x: "year",
          y: () => bandHeight / 2,
          title: d => `${d.source}\n${d.year}: ${d.value.toFixed(1)} mm`,
          fill: "white",
          stroke: "#64748b"
        })
      )
    ]
  });
}
```

<div class="card">
  ${resize((width) => horizonChart({width}))}
</div>

---

## 📅 Calendar Heatmap - Annual Rate of Change Patterns

This heatmap shows the annual rate of sea level change using **CSIRO Reconstructed GMSL** data. Each cell represents one year, organized by decades. The color indicates how much **sea level** rose or fell each year (blue = slower rise, red = faster rise) calculated by comparing consecutive year values. Values are in millimeters of sea level rise per year.

```js
// Prepare calendar data with rate of change
const calendarData = csiroData.slice(1).map((d, i) => {
  const prevValue = csiroData[i].value;
  const rate = d.value - prevValue;
  const decade = Math.floor(d.year / 10) * 10;
  const yearInDecade = d.year % 10;

  return {
    year: d.year,
    rate: rate,
    decade: decade,
    yearInDecade: yearInDecade,
    decadeLabel: `${decade}s`,
    absValue: d.value
  };
}).filter(d => d.year >= 1880 && d.year <= latestYear);
```

```js
function calendarHeatmap({width} = {}) {
  const cellSize = Math.min(40, (width - 120) / 14);
  const decades = Array.from(new Set(calendarData.map(d => d.decade))).sort();

  return Plot.plot({
    width,
    height: decades.length * (cellSize + 4) + 60,
    marginLeft: 80,
    marginRight: 40,
    marginTop: 30,
    marginBottom: 30,
    padding: 0,
    x: {
      label: null,
      domain: d3.range(0, 10),
      axis: "top",
      tickFormat: d => d
    },
    fy: {
      label: null,
      domain: decades.map(d => `${d}s`),
      tickFormat: d => d
    },
    color: {
      scheme: "RdBu",
      reverse: true,
      domain: d3.extent(calendarData, d => d.rate),
      legend: true,
      label: "Annual Change (mm)"
    },
    marks: [
      // Heatmap cells
      Plot.cell(calendarData, {
        x: "yearInDecade",
        fy: "decadeLabel",
        fill: "rate",
        inset: 2,
        rx: 4,
        tip: true,
        title: d => `${d.year}\nRate: ${d.rate >= 0 ? '+' : ''}${d.rate.toFixed(1)} mm/year\nTotal: ${d.absValue.toFixed(1)} mm`
      }),
      // Year labels for first and last of each decade
      Plot.text(calendarData.filter(d => d.yearInDecade === 0 || d.yearInDecade === 9), {
        x: "yearInDecade",
        fy: "decadeLabel",
        text: d => String(d.year),
        fontSize: 9,
        fill: "#1f2937",
        fontWeight: "bold",
        textAnchor: "middle",
        dy: 1
      })
    ]
  });
}
```

<div class="card">
  ${resize((width) => calendarHeatmap({width}))}
</div>

---

## Decade Comparison

This bar chart shows the average sea level for each decade. Data uses **EPA** for decades 1880-2009 and **CSIRO** for 2010-2019. The average is calculated by summing all year values within each decade and dividing by 10. All values are in millimeters.

```js
const decadeData = [
  {decade: "1880-1889", value: epaData.filter(d => d.year >= 1880 && d.year < 1890).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1890-1899", value: epaData.filter(d => d.year >= 1890 && d.year < 1900).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1900-1909", value: epaData.filter(d => d.year >= 1900 && d.year < 1910).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1910-1919", value: epaData.filter(d => d.year >= 1910 && d.year < 1920).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1920-1929", value: epaData.filter(d => d.year >= 1920 && d.year < 1930).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1930-1939", value: epaData.filter(d => d.year >= 1930 && d.year < 1940).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1940-1949", value: epaData.filter(d => d.year >= 1940 && d.year < 1950).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1950-1959", value: epaData.filter(d => d.year >= 1950 && d.year < 1960).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1960-1969", value: epaData.filter(d => d.year >= 1960 && d.year < 1970).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1970-1979", value: epaData.filter(d => d.year >= 1970 && d.year < 1980).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1980-1989", value: epaData.filter(d => d.year >= 1980 && d.year < 1990).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "1990-1999", value: epaData.filter(d => d.year >= 1990 && d.year < 2000).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "2000-2009", value: epaData.filter(d => d.year >= 2000 && d.year < 2010).reduce((sum, d) => sum + d.value, 0) / 10},
  {decade: "2010-2019", value: csiroData.filter(d => d.year >= 2010 && d.year < 2020).reduce((sum, d) => sum + d.value, 0) / 10},
];
```

```js
function decadeChart(data, {width} = {}) {
  return Plot.plot({
    title: "Average Sea Level by Decade",
    width,
    height: 350,
    marginLeft: 60,
    marginBottom: 80,
    x: {label: null, tickRotate: -45},
    y: {label: "Average Sea Level (mm)", grid: true},
    marks: [
      Plot.barY(data, {
        x: "decade",
        y: "value",
        fill: d => d.value < 25 ? "#059669" : d.value < 75 ? "#f59e0b" : "#dc2626",
        tip: true
      }),
      Plot.ruleY([0])
    ]
  });
}
```

<div class="card">
  ${resize((width) => decadeChart(decadeData, {width}))}
</div>

---

## Key Findings

We estimate the rise in global average sea level from satellite altimeter data for **1993–2009** and from coastal and island sea-level measurements from **1880 to 2009**.

For 1993–2009 and after correcting for glacial isostatic adjustment, the estimated rate of rise is **3.2 ± 0.4 mm year⁻¹** from the satellite data and **2.8 ± 0.8 mm year⁻¹** from the in situ data.

The global average sea-level rise from 1880 to 2009 is about **210 mm**. The linear trend from 1900 to 2009 is **1.7 ± 0.2 mm year⁻¹** and since 1961 is **1.9 ± 0.4 mm year⁻¹**.

There is considerable variability in the rate of rise during the twentieth century but there has been a **statistically significant acceleration** since 1880 and 1900 of **0.009 ± 0.003 mm year⁻²** and **0.009 ± 0.004 mm year⁻²**, respectively.

Since the start of the altimeter record in 1993, global average sea level rose at a rate near the **upper end of the sea level projections** of the Intergovernmental Panel on Climate Change's Third and Fourth Assessment Reports. However, the reconstruction indicates there was **little net change in sea level from 1990 to 1993**, most likely as a result of the volcanic eruption of **Mount Pinatubo in 1991**.

## References

- https://datahub.io/core/sea-level-rise
- https://github.com/datasets/sea-level-rise
- https://datahub.io/core/global-temp
- https://datahub.io/core/glacier-mass-balance
- https://www.epa.gov/climate-indicators/climate-change-indicators-sea-level
- https://www.cmar.csiro.au/sealevel/sl_hist_last_decades.html

---

**Note:** This shows global mean sea level. Local changes vary due to land subsidence, ocean currents, and regional climate patterns.
