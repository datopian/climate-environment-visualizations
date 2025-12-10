# Climate Data Portal

Interactive data visualization portal for exploring climate change indicators including global warming, atmospheric CO₂, sea level rise, and emissions by nation. Built with [Observable Framework](https://observablehq.com/framework/) and [PortalJS](https://www.portaljs.com/).

## Live Demo

**[climate.portaljs.com](https://climate.portaljs.com)**

## Dashboards

### Global Warming Effects
Track global temperature anomalies and warming trends:
- Temperature anomalies from 1880 to present
- Multi-decade warming comparisons
- Historical context and projections

### Atmospheric CO₂ Monitoring
The Keeling Curve - continuous CO₂ measurements from Mauna Loa:
- Monthly CO₂ concentrations since 1958
- Critical threshold visualization (350, 450 PPM)
- Annual rate of increase trends
- Decade average comparisons

### Sea Level Rise Dashboard
Monitor global sea level changes:
- Sea level measurements and trends
- Rate of change analysis
- Regional comparisons

### Global CO₂ Emissions by Nation
Cumulative emissions by country from 1751-2020:
- Emissions breakdown by fuel source (solid, liquid, gas, cement)
- Top emitters treemap visualization
- Historical trends for major emitters

## Data Sources

- **Temperature Data**: [NOAA National Centers for Environmental Information](https://www.ncei.noaa.gov/)
- **CO₂ Data**: [Scripps Institution of Oceanography](https://scripps.ucsd.edu/), NOAA Earth System Research Laboratory
- **Sea Level Data**: [NASA Sea Level Change Portal](https://sealevel.nasa.gov/)
- **Emissions Data**: [CDIAC](https://cdiac.ess-dive.lbl.gov/) via [DataHub](https://datahub.io/core/co2-fossil-by-nation)

## Getting Started

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) to view the portal.

## Project Structure

```
.
├─ src
│  ├─ data
│  │  ├─ co2-mm-mlo.csv              # Mauna Loa CO₂ measurements
│  │  ├─ fossil-fuel-co2-emissions-by-nation.csv  # Emissions by country
│  │  └─ ...                         # Other climate datasets
│  ├─ global-warming-dashboard.md    # Temperature anomalies dashboard
│  ├─ co2-monitoring.md              # Atmospheric CO₂ dashboard
│  ├─ sea-level-dashboard.md         # Sea level rise dashboard
│  ├─ co2-emissions-nations.md       # Emissions by nation dashboard
│  ├─ index.md                       # Home page
│  └─ style.css                      # Global styles
├─ observablehq.config.js            # App configuration
└─ package.json
```

## Commands

| Command         | Description                                 |
| --------------- | ------------------------------------------- |
| `npm install`   | Install or reinstall dependencies           |
| `npm run dev`   | Start local preview server                  |
| `npm run build` | Build your static site, generating `./dist` |
| `npm run clean` | Clear the local data loader cache           |
