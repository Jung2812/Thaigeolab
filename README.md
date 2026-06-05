# ThaiGeolab · Bangkok District Atlas

**An interactive, infographic-first legal and spatial atlas of Bangkok's 50 districts — built for architects, urban designers, and land professionals.**

> _"What can I actually build here?"_ — this tool answers that question at a glance.

---

## Overview

ThaiGeolab Bangkok District Atlas is a single-file, zero-dependency HTML application that layers spatial, legal, demographic, and land-use data across every district (เขต) in Bangkok. It is designed as a decision-support tool for design and planning professionals who need to quickly understand land constraints before engaging with raw government databases.

This is the Bangkok module of a larger planned national atlas covering provinces across Thailand where special laws, zoning overlays, and land title restrictions significantly affect what can be designed or built.

---

## Live Demo

> Deploy to GitHub Pages, Netlify, or Vercel — no server required. See [Deployment](#deployment) below.

---

## Features

### Map & Navigation
- **SVG district map** — all 50 Bangkok districts rendered from BMA GIS geometry (EPSG:4326), with smooth click-to-zoom drill-down per district
- **Sub-district drill-down** — click any district to zoom in and reveal sub-district (แขวง) boundaries and labels
- **4 display modes** switchable via layer toggle:
  - `Default` — neutral topographic view
  - `Zoning` — Bangkok Comprehensive Plan 2013 colour palette (ผังเมืองรวม พ.ศ. 2556), 20+ zone codes
  - `Density` — population density choropleth
  - `Land Value` — Treasury Department appraisal values 2566–2569 (บาท/ตร.ว.)
- **Search** — bilingual fuzzy search (Thai/English) with keyboard navigation
- **Zoom** — mouse-wheel + zoom buttons + reset; map pans on drag
- **Tooltip** — hover any district for instant zone, area, population, density summary
- **Compass + scale bar**
- **Light / Dark mode** toggle with smooth animated transition

### District Dossier (right panel)
Each district click populates a structured data panel:

| Section | Content |
|---|---|
| **Header** | District name (EN + TH), district code, primary zone classification |
| **Zone chips** | All assigned zone codes for the district |
| **Key stats** | Area (km²), population density (/km²) |
| **Land Value** | Treasury appraisal value for main road frontage (กรมธนารักษ์ 2566-2569) |
| **Kind of Land** | Animated bar chart breakdown of land-use categories (residential, commercial, green, etc.) |
| **Legal Layer** | Special laws, setbacks, height limits, heritage overlays, EIA triggers applicable to the zone |
| **Population** | Registered population, male/female ratio bar, BMA Census source |
| **POI counts** | Temples, schools, health facilities, communities — each tappable to open named list modal |
| **Notable Places** | Key landmarks and institutions |
| **Permitted Uses Matrix** | 22-category use table per zone code; colour-coded: green (permitted) / amber (conditional) / red (restricted/prohibited) — sourced from BMA ministerial regulations |

### Permitted Uses Matrix
The most design-relevant feature: for any district's primary zone, the atlas renders a full grid of 22 building/use types (residential, hi-rise condo, commercial, office tower, hotel, hospital, clinic, school, university, park, museum, temple, restaurant, retail, market, factory, warehouse, gas station, embassy, government office, farm, sports complex) with three permission levels based on the Bangkok Comprehensive Plan 2013.

---

## Data Sources

| Dataset | Source | Notes |
|---|---|---|
| District boundaries | BMA Open Portal / OpenGISData-Thailand | EPSG:4326 |
| Zoning (ผังเมืองรวม) | Bangkok Comprehensive Plan 2013 | 20+ zone codes |
| Land appraisal values | กรมธนารักษ์ (Treasury Department) | Round 2566-2569 |
| Population / Census | BMA Census 2024 | Registered population |
| Permitted use regulations | BMA Ministerial Regulations | Per zone class |
| POI data (temples, schools, hospitals) | BMA Open Portal | Per district counts + names |

---

## File Structure

This is a **single self-contained HTML file** — all CSS, JavaScript, SVG geometry, and data are embedded inline. No external CDN, no API calls, no build step required.

```
index.html              ← entire application (CSS + JS + data + SVG)
```

For future multi-province expansion, the recommended structure is:

```
/
├── index.html                  ← Province selector / homepage
├── bangkok/
│   └── index.html              ← Bangkok atlas (this file)
├── nakhon-phanom/
│   └── index.html              ← Nakhon Phanom atlas (planned)
└── data/
    ├── bangkok-districts.json  ← Extracted district data (for modular updates)
    └── land-values.json        ← Appraisal values (updated per Treasury cycle)
```

---

## Deployment

### GitHub Pages (recommended — free)

1. Fork or clone this repository
2. Rename `bangkok_atlas.html` → `index.html`
3. Go to **Settings → Pages → Source: main branch / root**
4. Your atlas is live at `https://yourusername.github.io/thaigeolab`

To update content: edit `index.html` directly in GitHub's editor, commit, and the live site updates within ~1 minute.

### Netlify (drag-and-drop option)

1. Go to [netlify.com](https://netlify.com)
2. Drag `index.html` onto the deploy area
3. Done — instant live URL, free tier

### Custom domain

Any static host works (Cloudflare Pages, Vercel, shared hosting). Point your domain's DNS to the host. No server-side logic is required.

---

## How to Update District Data

District data lives in the `DATA.districts` array inside the HTML file (search for `const DATA`). Each entry follows this schema:

```js
{
  id: "pathumwan",
  name_en: "Pathumwan",
  name_th: "ปทุมวัน",
  dcode: "10",
  area_km2: 8.9,
  pop: 41234,
  male: 19500,
  female: 21734,
  density: 4633,
  primary_zone: "R9",
  zones: ["R9", "W1", "K1"],
  cx: 512, cy: 380,           // label position on SVG
  path: "M ...",              // SVG path string
  land: [
    { n: "Commercial", code: "พ.", v: 55 },
    { n: "Residential", code: "ย.", v: 20 },
    ...
  ],
  highlights: ["Siam Square", "MBK Center", "Chulalongkorn University"],
  no_temple: 4,
  no_sch: 12,
  no_hos: 3,
  no_health: 8,
  no_commu: 15
}
```

To update land appraisal values, edit the `LAND_VALUE` object (one entry per district Thai name, value in บาท/ตร.ว.).

---

## Roadmap

- [ ] Nakhon Phanom province atlas (12 districts) — in prototype
- [ ] Province selector homepage
- [ ] Chiang Mai, Phuket, Chonburi atlases
- [ ] GeoJSON boundary swap (GADM/HDX) for production-accuracy district shapes
- [ ] Special law filter: coastal setbacks, heritage zones, altitude restrictions, EIA-required zones
- [ ] Land title type overlay (โฉนด / นส.3 / สปก. / ส.ป.ก.)
- [ ] Export to PDF dossier per district

---

## Context & Motivation

Thai government land data is fragmented across multiple agencies — the Land Department, BMA, Royal Forest Department, ALRO, Treasury Department, DNP — making it difficult for architects and designers to answer basic planning questions without consulting each separately.

Existing tools (Longdo Map, the Land Department portal) are oriented toward real estate buyers and have significant coverage gaps for design-relevant legal layers (setbacks, FAR limits, heritage overlays, EIA thresholds).

ThaiGeolab's goal is to consolidate these layers into a single, visually legible interface aimed specifically at design and planning professionals.

---

## License

Data sourced from Thai government open portals. Application code MIT licensed.  
Please verify all zoning and regulatory data against official sources before use in professional practice.

> โปรดตรวจทานข้อมูลก่อนนำไปประกอบการใช้งานจริงในงานวิชาชีพ
