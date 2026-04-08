# Broadband Provider Coverage Comparison  
## FCC vs FiberLocator Data – Texas Case Study (Odessa & Wichita Falls)

## Project Overview

This project analyzes broadband provider coverage using FCC Broadband Data Collection (BDC) datasets and compares it with infrastructure data from the FiberLocator API.

The goal is to identify differences, overlaps, and gaps between **reported broadband availability (FCC)** and **actual network infrastructure (FiberLocator)** in selected Texas cities.

## Research Goal

The primary objective is to compare broadband provider coverage across datasets to understand:

- Provider presence by location
- Differences between reported service and physical infrastructure
- Geographic patterns of broadband availability

## Research Questions

1. Which broadband carriers are available in each city based on FCC data?
2. Which broadband carriers are available based on FiberLocator data?
3. How do FCC and FiberLocator provider lists compare?
4. Are there providers present in FCC but missing in FiberLocator?
5. Are there providers present in FiberLocator but not in FCC?
6. How many providers are available per geographic area (H3 hexagon)?
7. Are there areas where FCC shows coverage but FiberLocator does not?
8. Are there areas where FiberLocator shows infrastructure but FCC does not?

## Study Areas

Since the FCC dataset does not include city names, counties were used as geographic proxies:

- **Ector County (48135)** → Odessa  
- **Midland County (48329)** → Odessa metro area  
- **Wichita County (48485)** → Wichita Falls  

Filtering was performed using the first five digits of the `block_geoid`.

## Data Sources

### FCC Broadband Data Collection (BDC) from Texas.
- Fiber-to-the-premises dataset  
- Cable dataset  

Variables used include:
- `brand_name` (provider)
- `technology`
- `business_residential_code`
- `block_geoid`
- `h3_res8_id`

### FiberLocator Data
Accessed via API and/or GeoJSON:
- Metro fiber networks  
- Long-haul routes  
- Connected buildings  

Used to represent physical infrastructure.

## Methodology

### 1. Data Ingestion
- Loaded FCC fiber and cable datasets from Google Drive
- Combined datasets using concatenation for unified analysis

### 2. Data Cleaning & Preparation
-  Used string conversion on `block_geoid` during filtering to extract the first five digits (county FIPS codes)
- Extracted county FIPS from `block_geoid`
- Filtered dataset to selected counties
- Removed residential-only records (`R`)
- Focused on:
  - Business (`B`)
  - Business/Residential (`X`)


### 3. Exploratory Data Analysis (EDA)
- Counted providers using `brand_name`
- Analyzed distribution of technologies
- Evaluated business vs residential composition
- Generated summary tables and visualizations


### 4. Spatial Aggregation (H3)

Data was grouped by `h3_res8_id` to create spatial summaries:

- Number of locations
- Unique providers (`unique_brands`)
- Provider count (`unique_brands_count`)
- Technologies present

---

### 5. Geometry Creation
- Converted H3 indices into polygon geometries
- Built GeoDataFrames using GeoPandas and Shapely
- Exported GeoJSON for mapping

---

### 6. Visualization

Maps were created using Folium:

#### FCC Maps
- Show provider density and availability using H3 hexagons

#### FiberLocator Maps
- Overlay infrastructure:
  - Metro networks
  - Long-haul routes
  - Connected buildings

---

### 7. Comparison Analysis

FCC and FiberLocator data were compared to identify:

- Overlapping providers
- Missing providers in each dataset
- Spatial mismatches between availability and infrastructure

---

## 🔍 Key Observations

- Broadband availability varies significantly across hexagons
- Provider presence is concentrated in specific geographic clusters
- Some areas show infrastructure presence but limited FCC-reported providers
- Differences exist between reported coverage and physical network assets

---

## 🛠️ Tools & Technologies

- Python (Google Colab)
  - Pandas
  - GeoPandas
  - Folium
  - H3
  - Requests
  - Shapely
  - NumPy
  - Matplotlib

---

## 📁 Repository Contents

- `BDC_TEXAS_2026.ipynb` → Main analysis notebook  
- `README.md` → Project documentation  

---

## ⚠️ Limitations

- County-level filtering may include areas outside exact city boundaries  
- FCC and FiberLocator datasets may not fully align  
- Dataset size impacts processing performance  
- FiberLocator data depends on API availability  

---

## 🚀 Future Work

- Extend analysis to Kingston, New York  
- Incorporate speed tiers and performance metrics  
- Improve map styling and interactivity  
- Perform deeper statistical comparisons across regions  

---

