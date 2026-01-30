
# Sentinel-2 Wind Erosion Proxy for Mining Tailings (Google Earth Engine)

Credits & Institutional Affiliation

This project was developed within the framework of the geospatial analytics and digital transformation initiatives of:

Servicio Nacional de Geología y Minería (SERNAGEOMIN), Chile
Subdepartamento de Analítica Territorial

Core Development Team

Hugo Neira

Elizabeth Sandoval 

Andrés Gómez

Institutional Context

The workflow was created as part of SERNAGEOMIN’s efforts to strengthen:

operational environmental monitoring of mining infrastructures

satellite-based risk assessment methodologies

data-driven territorial decision support

with a particular focus on tailings facilities and geotechnical environmental risk management.

📖 How to Cite

If you use this workflow in academic research, technical reports, or operational monitoring, please cite as:

Neira, H., Sandoval, E., & Gómez, A. (2026). Sentinel-2 wind erosion proxy for dust emission assessment over mining tailings using Google Earth Engine. GitHub repository. https://github.com/USERNAME/sentinel2-wind-erosion-proxy-tailings

## Overview

This repository provides a Google Earth Engine (GEE) workflow for mapping **wind erosion susceptibility and dust emission potential over mining tailings** using Sentinel-2 surface reflectance imagery.

The method integrates spectral indicators of:

* vegetation cover (NDVI)
* surface moisture (NDMI)
* exposed bare soil (Bare Soil Index – BSI)

to derive a **heuristic proxy of surface erodibility and aeolian sediment mobilization**.

The tool produces:

* spatial maps of dust emission potential
* erosion-prone surface masks
* annual temporal statistics
* interactive visualization by year
* key performance indicators (KPIs) for erosion-susceptible areas

designed for operational monitoring of tailings facilities and disturbed mining surfaces.

---

## Methodological Concept

Wind erosion and dust emission from tailings are favored when surfaces exhibit:

* low or absent vegetation cover
* low surface moisture content
* high exposure of fine bare sediments

This workflow captures these conditions using multispectral Sentinel-2 data.

### Spectral Indices

| Index | Formula                                                         | Physical meaning   |
| ----- | --------------------------------------------------------------- | ------------------ |
| NDVI  | (NIR − Red) / (NIR + Red)                                       | Vegetation density |
| NDMI  | (NIR − SWIR1) / (NIR + SWIR1)                                   | Surface moisture   |
| BSI   | ((SWIR1 + Red) − (NIR + Blue)) / ((SWIR1 + Red) + (NIR + Blue)) | Bare soil exposure |

---

### Dust Emission Potential Proxy

The proxy is computed as:

```
DUST_POT = BSI × (1 − NDVI) × (1 − normalized NDMI)
```

Where:

* high BSI increases susceptibility
* low NDVI increases exposure
* low NDMI increases dryness

The resulting surface highlights areas with **high potential for aeolian sediment mobilization**.

---

### Erodible Surface Mask

A binary erosion-prone mask is generated using threshold criteria:

* NDVI ≤ vegetation threshold
* NDMI ≤ moisture threshold
* BSI ≥ bare soil threshold

This identifies surfaces meeting physical conditions for wind erosion.

---

## Data Source

* **Sentinel-2 Surface Reflectance (COPERNICUS/S2_SR_HARMONIZED)**
* Cloud masking using Scene Classification Layer (SCL)
* Spatial resolution: 10–20 m

---

## Main Outputs

### Spatial Products

* RGB surface composite (annual and multi-year median)
* NDVI, NDMI, BSI maps
* Dust emission potential heatmap (DUST_POT)
* Erosion-prone surface mask with visual outlines

### Temporal Analytics

* Annual erosion-prone area (hectares)
* Annual mean dust potential proxy
* Interactive time-series charts

### Interactive Dashboard (GEE UI)

* Year selector
* Dynamic map update
* KPIs:

  * erosion-prone surface area
  * mean dust proxy value
* Adaptive charts

---

## Key Parameters (Where to Customize)

### 1. Region of Interest (Tailings Area)

```javascript
var roi = ee.FeatureCollection("YOUR_ASSET_OR_DRAWN_POLYGON");
```

Replace with:

* your GEE asset
* or manually drawn polygon named `roi`

---

### 2. Analysis Period

```javascript
var start = '2019-01-01';
var end   = '2025-12-31';
```

Modify to:

* focus on specific operational periods
* analyze pre/post mitigation actions
* extend monitoring windows

---

### 3. Cloud Filtering

```javascript
var maxCloud = 40;
```

Typical values:

* 20–30 for strict quality
* 40–60 for arid regions with limited data

---

### 4. Erosion Thresholds (Core Method Controls)

```javascript
var thrNDVI = 0.20;   // vegetation
var thrNDMI = 0.00;   // moisture
var thrBSI  = 0.10;   // bare soil
```

Recommended tuning:

| Parameter | Lower → stricter        | Higher → more permissive  |
| --------- | ----------------------- | ------------------------- |
| NDVI      | less vegetation allowed | more vegetation allowed   |
| NDMI      | drier only              | include moist surfaces    |
| BSI       | only highly bare        | include moderate exposure |

These should be calibrated by:

* local climate
* tailings material
* field observations
* dust event history

---

### 5. Visualization Scaling

Dust proxy is automatically scaled using ROI percentiles:

```javascript
ee.Reducer.percentile([2, 98])
```

This enhances contrast and avoids extreme outliers.

---

## Suggested Validation Approaches

For operational deployments:

* Compare with known dust events
* Cross with wind speed datasets
* Relate to moisture sensors (if available)
* Compare against field erosion observations

---

## Applications

* Tailings environmental risk monitoring
* Dust mitigation effectiveness tracking
* Early warning of erosion-prone surfaces
* Regulatory oversight
* Environmental impact assessment
* Mine closure monitoring

---

## Limitations

* Proxy-based (not direct dust flux measurement)
* Sensitive to threshold calibration
* Does not explicitly model wind dynamics
* Best used as relative risk indicator

---

## Requirements

* Google Earth Engine account
* GEE Code Editor environment

---

## How to Run

1. Open Google Earth Engine Code Editor
2. Paste `sentinel2_wind_erosion_proxy_tailings.js`
3. Load or draw ROI
4. Run script
5. Use UI controls for annual analysis

---

## Credits

Developed by:

**SERNAGEOMIN – Subdepartamento de Analítica Territorial**
Hugo Neira, Elizabeth Sandoval, Andrés Gómez

