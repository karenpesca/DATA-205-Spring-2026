# Broadband Provider Coverage Comparison  
## FCC vs FiberLocator Data – Texas Case Study (Odessa & Wichita Falls)

## 1. Project Overview

This project analyzes broadband provider coverage using FCC Broadband Data Collection (BDC) datasets and compares it with data from the FiberLocator API.

The goal is to identify differences, overlaps, and gaps between FCC-reported broadband availability and network infrastructure data provided by FiberLocator in selected Texas cities.

### Research Goal

The primary objective is to compare broadband provider coverage across datasets to understand:

- Provider presence by location
- Differences between reported service and physical infrastructure
- Geographic patterns of broadband availability

### Research Questions

1. Which broadband carriers are available in each city based on FCC data?
Method: Extract brand_name, group by H3 hexagon, and identify unique providers.
2. Which broadband carriers are available in each city based on FiberLocator data?
Method: Use the FiberLocator API to retrieve provider data and list carriers per city.
3. How do FCC and FiberLocator provider lists compare?
Method: Compare provider lists from both datasets to identify overlaps.
4. Are there providers present in FCC data but missing in FiberLocator?
Method: Perform a set comparison to identify providers that appear only in FCC data.
5. Are there providers present in FiberLocator but not in FCC data?
Method: Compare both datasets to identify additional providers from FiberLocator.
6. How many providers are available per hexagon (FCC data)?
Method: Calculate unique_brands_count and visualize provider density using maps.
7. Are there geographic areas where FCC shows coverage but FiberLocator does not?
Method: Compare spatial coverage using H3 hexagons and identify areas present in FCC but missing in FiberLocator.
8. Are there geographic areas where FiberLocator shows coverage but FCC does not?


## 2. Methodology

Since the FCC dataset does not include city names, counties were used as geographic proxies:

- **Ector County (48135)** → Odessa  
- **Midland County (48329)** → Odessa metro area  
- **Wichita County (48485)** → Wichita Falls  

Filtered data using the first five digits of the "block_geoid" variable.

### Tools & Technologies

- Python (Google Colab)
  - Pandas
  - GeoPandas
  - Folium
  - H3
  - Requests
  - Shapely
  - NumPy
  - Matplotlib

## Data Ingestion

This project uses both **static data** and **live data**:

#### Static Data (FCC)

- Source: FCC Broadband Data Collection (BDC)  
- Format: Csv files  
- Files used: Fiber-to-the-premises and Cable dataset from Texas.

Variables used include:
- "brand_name" (provider)
- "technology"
- "business_residential_code"
- "block_geoid"
- "h3_res8_id"

#### Live Data (FiberLocator API)

- Source: FiberLocator  
- Format: API / GeoJSON

Accessed via API and/or GeoJSON:
- Metro fiber networks  
- Long-haul routes  
- Connected buildings 

FCC fiber and cable datasets were loaded from Google Drive and combined using concatenation for a unified analysis. The FiberLocator API was accessed using username and password credentials provided by FiberLocator.


## 3. Data Cleaning & Preparation
-  Used string conversion on "block_geoid" during filtering to extract the first five digits (county FIPS codes)
- Extracted county FIPS from "block_geoid"
- Filtered dataset to selected counties
- Removed residential-only records ("R")
- Focused on:
  - Business ("B")
  - Business/Residential ("X")


## 4. Exploratory Data Analysis (EDA)
- Counted providers using "brand_name"
- Analyzed distribution of technologies
- Evaluated business vs residential composition
- Generated summary tables and visualizations (Bar chart and maps)


### Spatial Aggregation (H3)

Data was grouped by "h3_res8_id" to create spatial summaries:

- Number of locations
- Unique providers ("unique_brands")
- Provider count ("unique_brands_count")
- Technologies present

### Geometry Creation
- Converted H3 indices into polygon geometries
- Built GeoDataFrames using GeoPandas and Shapely
- Exported GeoJSON for mapping

### 5. Visualization

Visualizations were created using Folium and Matplotlib to support the analysis.

#### Bar Chart (FCC Data)  

- Shows the top broadband providers in Texas (Odessa y Wichita Fall) based on number of locations  
- Used as an initial step to understand provider distribution before spatial analysis

  
#### FCC Maps
- Show provider density and availability using H3 hexagons

#### FiberLocator Maps
- Overlay infrastructure:
  - Metro networks
  - Long-haul routes
  - Connected buildings


## 6. Comparison Analysis

FCC and FiberLocator data were compared to identify:

- Overlapping providers
- Missing providers in each dataset
- Spatial mismatches between availability and infrastructure


### Key Observations

- Broadband availability varies significantly across hexagons
- Provider presence is concentrated in specific geographic clusters
- Some areas show infrastructure presence but limited FCC-reported providers
- Differences exist between reported coverage and physical network assets


## Repository Contents

- `BDC_TEXAS_2026.ipynb`= Main analysis notebook  
- `README.md`= Project documentation  


## Limitations

- County-level filtering may include areas outside exact city boundaries  
- FCC and FiberLocator datasets may not fully align  
- Dataset size impacts processing performance  
- FiberLocator data depends on API availability  

## 7.  Exploratory Data Analysis (EDA) Findings  

Initial exploratory analysis identified differences between FCC data and FiberLocator data across the selected regions. Based on the first visualization (bar chart) discussed during the meeting with FiberLocator, it was observed that in some areas the FCC dataset reported a higher number of providers, while in others FiberLocator showed more detailed network infrastructure coverage.

Variation in provider concentration across H3 hexagons was also observed. Some areas showed multiple providers, indicating higher competition, while others had limited providers, suggesting potential service gaps. This pattern was verified using the FiberLocator API by reviewing coverage in specific locations.

These results indicate that FCC data and FiberLocator data capture different aspects of broadband coverage, which can lead to inconsistencies when comparing availability and infrastructure.

Further analysis will focus on examining H3 hexagons in more detail and reviewing differences in data structure and coverage between both datasets.

## Future steps

- Extend analysis to Kingston, New York   
- Improve map styling and interactivity  
- Perform deeper statistical comparisons across cities.



