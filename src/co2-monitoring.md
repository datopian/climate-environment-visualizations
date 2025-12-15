---
title: Atmospheric CO₂ Monitoring Dashboard
toc: false
---

<div class="portaljs-banner">
  <div class="portaljs-banner-content">
    <span class="portaljs-banner-icon">🌀</span>
    <div class="portaljs-banner-text">
      <p class="portaljs-banner-title">Create beautiful data portals with PortalJS</p>
      <p class="portaljs-banner-description"> The open-source framework for building data catalogs, dashboards, and visualizations.</p>
    </div>
  </div>
  <a href="https://www.portaljs.com/" target="_blank" rel="noopener noreferrer" class="portaljs-banner-cta">
    Get Started Free <span class="portaljs-banner-cta-arrow">→</span>
  </a>
</div>

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
  min-height: 280px;
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

.positive { color: #dc2626; }
.negative { color: #059669; }

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
  grid-template-columns: repeat(3, 1fr);
  gap: 0;
  overflow-anchor: none !important;
  contain: layout;
}

.chart-grid .chart-container {
  border: none;
  border-radius: 0;
  position: relative;
}

.chart-grid .chart-container:not(:last-child)::after {
  content: "";
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  width: 1px;
  background: #e5e7eb;
}

.chart-large {
  min-height: 380px !important;
  border-left: none !important;
  border-right: none !important;
  border-top: none !important;
  border-bottom: none !important;
  border-radius: 0 !important;
  margin-bottom: 0;
  position: relative;
}

.chart-large::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: #e5e7eb;
}

.chart-wide {
  grid-column: 1 / -1;
}

@media (max-width: 1200px) {
  .chart-grid {
    grid-template-columns: 1fr 1fr;
  }
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
  }

  .chart-grid .chart-container:not(:last-child)::after {
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
  <h1>Atmospheric CO₂ Monitoring Dashboard</h1>
  <p>The Keeling Curve - Continuous atmospheric CO₂ measurements from Mauna Loa Observatory, Hawaii (1958-Present)</p>
</div>

```js
const co2Raw = FileAttachment("data/co2-mm-mlo.csv").csv({typed: true});
const co2Data = (await co2Raw).map(d => ({
  date: new Date(d.Date),
  year: d["Decimal Date"],
  yearInt: Math.floor(d["Decimal Date"]),
  month: new Date(d.Date).getMonth() + 1,
  average: +d.Average,
  interpolated: +d.Interpolated,
  trend: +d.Trend > 0 ? +d.Trend : null
})).filter(d => !isNaN(d.average) && d.average > 0);

const latestReading = co2Data[co2Data.length - 1];
const firstReading = co2Data[0];
const increase = latestReading.average - firstReading.average;
const yearsElapsed = latestReading.year - firstReading.year;
const avgRate = increase / yearsElapsed;
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
const dashboardYears = d3.range(1958, Math.floor(latestReading.year) + 1);
const selectedDashboardYear = view(Scrubber(dashboardYears, {
  delay: 100,
  loop: false,
  initial: dashboardYears.length - 1,
  autoplay: false,
  format: d => String(d)
}));
```

  </div>

```js
const yearData = co2Data.filter(d => d.yearInt === selectedDashboardYear);
const yearAvg = yearData.length > 0 ? d3.mean(yearData, d => d.average) : firstReading.average;
const increaseFromStart = yearAvg - firstReading.average;
const yearsFromStart = selectedDashboardYear - Math.floor(firstReading.year);
const rateToDate = yearsFromStart > 0 ? increaseFromStart / yearsFromStart : 0;

const filteredData = co2Data.filter(d => d.yearInt <= selectedDashboardYear);
const recentYears = filteredData.filter(d => d.yearInt >= selectedDashboardYear - 10);
const recentRate = recentYears.length > 10 ? (recentYears[recentYears.length - 1].average - recentYears[0].average) / 10 : 0;
```

  <div class="stat-card">
    <div class="stat-label">Current CO₂</div>
    <div class="stat-value positive">${yearAvg.toFixed(1)}</div>
    <div class="stat-change">PPM (${selectedDashboardYear})</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Increase</div>
    <div class="stat-value positive">+${increaseFromStart.toFixed(1)}</div>
    <div class="stat-change">PPM Since 1958</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Average Rate</div>
    <div class="stat-value positive">${rateToDate.toFixed(2)}</div>
    <div class="stat-change">PPM/Year</div>
  </div>
  <div class="stat-card">
    <div class="stat-label">Recent Trend</div>
    <div class="stat-value positive">${recentRate > 0 ? recentRate.toFixed(2) : avgRate.toFixed(2)}</div>
    <div class="stat-change">PPM/Year (10yr)</div>
  </div>
  <div class="insights-section">
    <h3>Key Insights</h3>
    <ul>
      <li><strong>Safe Limit (350 PPM):</strong> Exceeded ~1990, we are now in the climate danger zone</li>
      <li><strong>Point of No Return (450 PPM):</strong> Approaching in ~10-15 years at current rate, triggers irreversible tipping points</li>
      <li><strong>Pre-industrial:</strong> CO₂ was ~280 PPM for 10,000+ years before industrialization</li>
      <li><strong>Current Level:</strong> Now above ${latestReading.average.toFixed(0)} PPM - causing record heat and extreme weather</li>
      <li><strong>Acceleration:</strong> Rate of increase has nearly doubled from 1960s to today</li>
      <li><strong>Indoor Risk:</strong> At 1,000+ PPM cognitive function declines - rising outdoor levels make indoor air harder to keep fresh</li>
    </ul>
    <div class="data-sources">
      <strong>Data:</strong> <a href="https://datahub.io/core/co2-ppm" target="_blank" rel="noopener noreferrer">Scripps Institution of Oceanography</a>, NOAA Earth System Research Laboratory
    </div>
  </div>
  </div>

  <div class="main-content">

```js
function keelingCurve({width} = {}) {
  const isMobile = width < 640;
  const height = isMobile ? 320 : 340;
  const marginTop = 20;
  const marginRight = isMobile ? 35 : 45;
  const marginBottom = isMobile ? 50 : 50;
  const marginLeft = isMobile ? 50 : 60;

  const xScale = d3.scaleLinear()
    .domain([1958, Math.ceil(latestReading.year)])
    .range([marginLeft, width - marginRight]);

  const yScale = d3.scaleLinear()
    .domain([310, Math.max(435, latestReading.average + 5)])
    .range([height - marginBottom, marginTop])
    .nice();

  const line = d3.line()
    .x(d => xScale(d.year))
    .y(d => yScale(d.average))
    .curve(d3.curveCatmullRom);

  const container = d3.create("div")
    .style("position", "relative");

  const svg = container.append("svg")
    .attr("width", width)
    .attr("height", height)
    .attr("viewBox", [0, 0, width, height])
    .attr("style", "max-width: 100%; height: auto; background: white; display: block;");

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
    .call(d3.axisLeft(yScale)
      .tickFormat("")
      .tickSize(-(width - marginLeft - marginRight)))
    .call(g => g.select(".domain").remove())
    .call(g => g.selectAll(".tick line")
      .attr("stroke", "#e5e7eb")
      .attr("stroke-opacity", 0.7));

  svg.append("g")
    .attr("transform", `translate(0,${height - marginBottom})`)
    .call(d3.axisBottom(xScale).tickFormat(d3.format("d")).ticks(isMobile ? 5 : 10).tickSize(0).tickPadding(8))
    .call(g => g.select(".domain").attr("stroke", "#ccc"))
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "11px"));

  svg.append("g")
    .attr("transform", `translate(${marginLeft},0)`)
    .call(d3.axisLeft(yScale).ticks(isMobile ? 5 : 7).tickSize(0).tickPadding(6).tickFormat(d => `${d}`))
    .call(g => g.select(".domain").attr("stroke", "#ccc"))
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "11px"));

  svg.append("text")
    .attr("x", marginLeft)
    .attr("y", marginTop - 5)
    .attr("fill", "currentColor")
    .attr("font-size", isMobile ? "9px" : "10px")
    .text("PPM");

  svg.append("defs").append("linearGradient")
    .attr("id", "areaGradient")
    .attr("x1", "0%").attr("y1", "0%")
    .attr("x2", "0%").attr("y2", "100%")
    .selectAll("stop")
    .data([
      {offset: "0%", color: "#0ea5e9", opacity: 0.3},
      {offset: "100%", color: "#22c55e", opacity: 0.1}
    ])
    .join("stop")
    .attr("offset", d => d.offset)
    .attr("stop-color", d => d.color)
    .attr("stop-opacity", d => d.opacity);

  const area = d3.area()
    .x(d => xScale(d.year))
    .y0(height - marginBottom)
    .y1(d => yScale(d.average))
    .curve(d3.curveCatmullRom);

  svg.append("path")
    .datum(filteredData)
    .attr("fill", "url(#areaGradient)")
    .attr("d", area);

  svg.append("path")
    .datum(filteredData)
    .attr("fill", "none")
    .attr("stroke", "#0ea5e9")
    .attr("stroke-width", 2.5)
    .attr("d", line);

  const hoverCircle = svg.append("circle")
    .attr("r", 5)
    .attr("fill", "#0ea5e9")
    .attr("stroke", "white")
    .attr("stroke-width", 2)
    .style("visibility", "hidden")
    .style("pointer-events", "none");

  svg.append("rect")
    .attr("x", marginLeft)
    .attr("y", marginTop)
    .attr("width", width - marginLeft - marginRight)
    .attr("height", height - marginTop - marginBottom)
    .attr("fill", "none")
    .attr("pointer-events", "all")
    .on("mousemove", function(event) {
      const [mx] = d3.pointer(event, svg.node());
      const year = xScale.invert(mx);

      const bisect = d3.bisector(d => d.year).left;
      const index = bisect(filteredData, year);
      const d0 = filteredData[index - 1];
      const d1 = filteredData[index];

      if (!d0 && !d1) return;

      const d = !d0 ? d1 : !d1 ? d0 : year - d0.year > d1.year - year ? d1 : d0;

      const dataX = xScale(d.year);
      const dataY = yScale(d.average);

      hoverCircle
        .attr("cx", dataX)
        .attr("cy", dataY)
        .style("visibility", "visible");

      const dateStr = d.date.toLocaleDateString('en-US', { month: 'short', year: 'numeric' });
      tooltip
        .html(`<strong>${dateStr}</strong><br/>CO₂: ${d.average.toFixed(2)} PPM`)
        .style("visibility", "visible");

      const tooltipNode = tooltip.node();
      const tooltipWidth = tooltipNode.offsetWidth;
      const tooltipHeight = tooltipNode.offsetHeight;

      let left = dataX + 15;
      let top = dataY - tooltipHeight / 2;

      if (left + tooltipWidth > width - marginRight) {
        left = dataX - tooltipWidth - 15;
      }
      if (top < marginTop) top = marginTop;
      if (top + tooltipHeight > height - marginBottom) top = height - marginBottom - tooltipHeight;

      tooltip
        .style("left", `${left}px`)
        .style("top", `${top}px`);
    })
    .on("mouseout", () => {
      tooltip.style("visibility", "hidden");
      hoverCircle.style("visibility", "hidden");
    });

  if (selectedDashboardYear >= 1958) {
    svg.append("line")
      .attr("x1", xScale(selectedDashboardYear))
      .attr("x2", xScale(selectedDashboardYear))
      .attr("y1", marginTop)
      .attr("y2", height - marginBottom)
      .attr("stroke", "#f59e0b")
      .attr("stroke-width", 2)
      .attr("stroke-dasharray", "4,4");

    const yearPoint = filteredData[filteredData.length - 1];
    if (yearPoint) {
      svg.append("circle")
        .attr("cx", xScale(yearPoint.year))
        .attr("cy", yScale(yearPoint.average))
        .attr("r", 6)
        .attr("fill", "#dc2626")
        .attr("stroke", "white")
        .attr("stroke-width", 2);
    }
  }

  svg.append("line")
    .attr("x1", marginLeft)
    .attr("x2", width - marginRight)
    .attr("y1", yScale(280))
    .attr("y2", yScale(280))
    .attr("stroke", "#22c55e")
    .attr("stroke-width", 1.5)
    .attr("stroke-dasharray", "4,3")
    .attr("opacity", 0.5);

  svg.append("text")
    .attr("x", isMobile ? marginLeft + 5 : marginLeft + 35)
    .attr("y", yScale(280) - 5)
    .attr("fill", "#22c55e")
    .attr("font-size", isMobile ? "9px" : "10px")
    .text("Pre-industrial (280)");

  const legend = svg.append("g")
    .attr("transform", `translate(${width / 2 - 80}, ${height - (isMobile ? 15 : 12)})`);

  legend.append("line")
    .attr("x1", 0)
    .attr("x2", isMobile ? 18 : 25)
    .attr("y1", 0)
    .attr("y2", 0)
    .attr("stroke", "#0ea5e9")
    .attr("stroke-width", 2.5);

  legend.append("text")
    .attr("x", isMobile ? 22 : 30)
    .attr("y", 4)
    .attr("fill", "currentColor")
    .attr("font-size", isMobile ? "10px" : "11px")
    .text("Monthly Average CO₂");

  return container.node();
}
```

```js
const yearlyAverage = d3.rollups(
  co2Data,
  v => d3.mean(v, d => d.average),
  d => d.yearInt
).map(([year, avg]) => ({ year, average: avg })).sort((a, b) => a.year - b.year);

const yearlyRates = yearlyAverage.slice(1).map((d, i) => ({
  year: d.year,
  rate: d.average - yearlyAverage[i].average
}));

const filteredRates = yearlyRates.filter(d => d.year <= selectedDashboardYear);
```

```js
function rateOfIncreaseChart({width} = {}) {
  const isMobile = width < 640;

  return Plot.plot({
    width,
    height: isMobile ? 260 : 240,
    marginLeft: isMobile ? 40 : 50,
    marginRight: isMobile ? 20 : 30,
    marginBottom: isMobile ? 45 : 40,
    style: {
      fontSize: isMobile ? "11px" : "13px"
    },
    x: {
      label: null,
      grid: true,
      ticks: isMobile ? 5 : 8,
      tickFormat: d => String(d)
    },
    y: {
      label: "Annual Increase (PPM/year)",
      grid: true
    },
    marks: [
      Plot.ruleY([0], {stroke: "#94a3b8", strokeDasharray: "2,2"}),
      Plot.line(filteredRates, {
        x: "year",
        y: "rate",
        stroke: "#dc2626",
        strokeWidth: 2,
        curve: "catmull-rom"
      }),
      Plot.dot(filteredRates, {
        x: "year",
        y: "rate",
        r: 3,
        fill: "transparent",
        stroke: "transparent",
        tip: true,
        title: d => `Year: ${d.year}\nIncrease: ${d.rate.toFixed(3)} PPM/year`
      }),
      Plot.ruleX([selectedDashboardYear], {stroke: "#f59e0b", strokeWidth: 2, strokeDasharray: "4,4"}),
      Plot.dot(filteredRates.filter(d => d.year === selectedDashboardYear), {
        x: "year",
        y: "rate",
        fill: "#dc2626",
        stroke: "white",
        strokeWidth: 2,
        r: 6
      })
    ]
  });
}
```

```js
const thresholds = [
  { level: 280, label: "Pre-industrial", status: "passed", color: "#22c55e", description: "Natural baseline for 10,000+ years" },
  { level: 350, label: "Safe Climate Limit", status: "passed", color: "#f59e0b", description: "Exceeded ~1990, entering danger zone" },
  { level: yearAvg, label: "Current Level", status: "current", color: "#dc2626", description: `${selectedDashboardYear} measurement` },
  { level: 450, label: "Point of No Return", status: "approaching", color: "#7f1d1d", description: "2°C warming, irreversible tipping points" }
];

const yearsTo450 = recentRate > 0 ? Math.ceil((450 - yearAvg) / recentRate) : null;
const projectedYear450 = yearsTo450 ? selectedDashboardYear + yearsTo450 : null;
```

```js
function thresholdsChart({width} = {}) {
  const isMobile = width < 640;
  const height = isMobile ? 280 : 260;
  const marginTop = 25;
  const marginRight = isMobile ? 15 : 20;
  const marginBottom = isMobile ? 40 : 35;
  const marginLeft = isMobile ? 45 : 55;

  const container = d3.create("div")
    .style("position", "relative");

  const svg = container.append("svg")
    .attr("width", width)
    .attr("height", height)
    .attr("viewBox", [0, 0, width, height])
    .style("background", "white")
    .style("display", "block");

  const yScale = d3.scaleLinear()
    .domain([250, 480])
    .range([height - marginBottom, marginTop]);

  const barWidth = Math.min(width * 0.35, 120);
  const barX = marginLeft + 20;

  svg.append("g")
    .attr("transform", `translate(${marginLeft},0)`)
    .call(d3.axisLeft(yScale).ticks(6).tickSize(0).tickPadding(6))
    .call(g => g.select(".domain").remove())
    .call(g => g.selectAll("text").attr("font-size", isMobile ? "10px" : "11px"));

  svg.append("text")
    .attr("x", marginLeft - 8)
    .attr("y", marginTop - 10)
    .attr("fill", "currentColor")
    .attr("font-size", isMobile ? "9px" : "10px")
    .attr("text-anchor", "end")
    .text("PPM");

  const zones = [
    { y0: 250, y1: 280, color: "#dcfce7", label: "" },
    { y0: 280, y1: 350, color: "#fef9c3", label: "" },
    { y0: 350, y1: 450, color: "#fed7aa", label: "" },
    { y0: 450, y1: 480, color: "#fecaca", label: "" }
  ];

  zones.forEach(zone => {
    svg.append("rect")
      .attr("x", barX)
      .attr("y", yScale(zone.y1))
      .attr("width", barWidth)
      .attr("height", yScale(zone.y0) - yScale(zone.y1))
      .attr("fill", zone.color)
      .attr("stroke", "#e5e7eb")
      .attr("stroke-width", 0.5);
  });

  svg.append("rect")
    .attr("x", barX)
    .attr("y", yScale(yearAvg))
    .attr("width", barWidth)
    .attr("height", yScale(280) - yScale(yearAvg))
    .attr("fill", "#dc2626")
    .attr("fill-opacity", 0.7);

  const thresholdLines = [
    { level: 280, label: "Pre-industrial (280)", color: "#22c55e" },
    { level: 350, label: "Safe Limit (350)", color: "#f59e0b" },
    { level: 450, label: "Point of No Return (450)", color: "#7f1d1d" }
  ];

  thresholdLines.forEach(t => {
    svg.append("line")
      .attr("x1", barX - 5)
      .attr("x2", barX + barWidth + 5)
      .attr("y1", yScale(t.level))
      .attr("y2", yScale(t.level))
      .attr("stroke", t.color)
      .attr("stroke-width", 2)
      .attr("stroke-dasharray", t.level === 450 ? "4,3" : "0");

    svg.append("text")
      .attr("x", barX + barWidth + 10)
      .attr("y", yScale(t.level) + 4)
      .attr("fill", t.color)
      .attr("font-size", isMobile ? "9px" : "10px")
      .attr("font-weight", "600")
      .text(t.label);
  });

  svg.append("line")
    .attr("x1", barX - 8)
    .attr("x2", barX + barWidth + 8)
    .attr("y1", yScale(yearAvg))
    .attr("y2", yScale(yearAvg))
    .attr("stroke", "#dc2626")
    .attr("stroke-width", 3);

  svg.append("circle")
    .attr("cx", barX - 12)
    .attr("cy", yScale(yearAvg))
    .attr("r", 5)
    .attr("fill", "#dc2626")
    .attr("stroke", "white")
    .attr("stroke-width", 2);

  svg.append("text")
    .attr("x", barX + barWidth + 10)
    .attr("y", yScale(yearAvg) + 4)
    .attr("fill", "#dc2626")
    .attr("font-size", isMobile ? "10px" : "11px")
    .attr("font-weight", "bold")
    .text(`Current: ${yearAvg.toFixed(1)} PPM`);

  if (projectedYear450 && projectedYear450 > selectedDashboardYear) {
    svg.append("text")
      .attr("x", barX + barWidth / 2)
      .attr("y", height - 8)
      .attr("text-anchor", "middle")
      .attr("fill", "#7f1d1d")
      .attr("font-size", isMobile ? "9px" : "10px")
      .attr("font-weight", "600")
      .text(`~${yearsTo450} years to 450 PPM (by ${projectedYear450})`);
  }

  return container.node();
}
```

```js
const decadeData = d3.rollups(
  co2Data,
  v => d3.mean(v, d => d.average),
  d => Math.floor(d.yearInt / 10) * 10
).map(([decade, avg]) => ({
  decade: `${decade}s`,
  decadeNum: decade,
  average: avg
})).sort((a, b) => a.decadeNum - b.decadeNum);

const filteredDecades = decadeData.filter(d => d.decadeNum <= Math.floor(selectedDashboardYear / 10) * 10);
```

```js
function decadeComparison({width} = {}) {
  const isMobile = width < 640;
  const currentDecade = Math.floor(selectedDashboardYear / 10) * 10;

  return Plot.plot({
    width,
    height: isMobile ? 260 : 240,
    marginLeft: isMobile ? 40 : 50,
    marginRight: isMobile ? 20 : 30,
    marginBottom: isMobile ? 60 : 55,
    style: {
      fontSize: isMobile ? "11px" : "13px"
    },
    x: {
      label: null,
      tickRotate: -45
    },
    y: {
      label: "Average CO₂ (PPM)",
      grid: true
    },
    marks: [
      Plot.barY(filteredDecades, {
        x: "decade",
        y: "average",
        fill: d => d.decadeNum === currentDecade ? "#f59e0b" : "#3b82f6",
        fillOpacity: d => d.decadeNum === currentDecade ? 1 : 0.7,
        stroke: d => d.decadeNum === currentDecade ? "white" : "none",
        strokeWidth: 2,
        tip: true,
        title: d => `${d.decade}\nAverage: ${d.average.toFixed(2)} PPM`
      }),
      Plot.ruleY([0])
    ]
  });
}
```

<div class="chart-container chart-large">
  <h3>The Keeling Curve - Atmospheric CO₂ (1958-Present)</h3>
  ${resize((width) => keelingCurve({width}))}
</div>

<div class="chart-grid">
  <div class="chart-container">
    <h3>Annual Rate of Increase</h3>
    ${resize((width) => rateOfIncreaseChart({width}))}
  </div>

  <div class="chart-container">
    <h3>Critical CO₂ Thresholds</h3>
    ${resize((width) => thresholdsChart({width}))}
  </div>

  <div class="chart-container">
    <h3>Decade Average Comparison</h3>
    ${resize((width) => decadeComparison({width}))}
  </div>
</div>

  </div>
</div>
